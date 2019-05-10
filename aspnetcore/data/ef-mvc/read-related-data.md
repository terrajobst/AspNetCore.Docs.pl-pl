---
title: 'Samouczek: Odczytywanie powiązanych danych — ASP.NET MVC z programem EF Core'
description: W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.
author: rick-anderson
ms.author: tdykstra
ms.date: 03/27/2019
ms.topic: tutorial
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 5b45ced4fe6ed220c9a63bc6480c8c8520b36ed4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900361"
---
# <a name="tutorial-read-related-data---aspnet-mvc-with-ef-core"></a>Samouczek: Odczytywanie powiązanych danych — ASP.NET MVC z programem EF Core

W poprzednim samouczku można wykonać modelu danych służbowych. W tym samouczku należy przeczytać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, którą będziesz pracować.

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dowiedz się, jak załadować dane pokrewne
> * Utwórz stronę kursów
> * Tworzenie strony instruktorów
> * Dowiedz się więcej o jawne ładowanie

## <a name="prerequisites"></a>Wymagania wstępne

* [Tworzenie złożonego modelu danych](complex-data-model.md)

## <a name="learn-how-to-load-related-data"></a>Dowiedz się, jak załadować dane pokrewne

Istnieje kilka sposobów oprogramowanie obiektowo-relacyjny mapowanie (ORM), takie jak Entity Framework można załadować powiązane dane do właściwości nawigacji jednostki:

* Wczesne ładowanie. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne. Możesz określić wczesne ładowanie w Entity Framework Core przy użyciu `Include` i `ThenInclude` metody.

  ![Przykład wczesne ładowanie](read-related-data/_static/eager-loading.png)

  Możesz pobrać dane w oddzielne zapytania, a EF termin "poprawki" właściwości nawigacji.  Oznacza to EF automatycznie doda jednostki pobrane oddzielnie, których one należą do właściwości nawigacji jednostek wcześniej pobrane. Dla zapytania, które pobiera dane powiązane, możesz użyć `Load` metody zamiast metody, która zwraca listę lub obiektów, takich jak `ToList` lub `Single`.

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

* Jawne ładowanie. Podczas odczytywania jednostki powiązane dane nie są pobierane. Jeśli jest konieczne, piszesz kod, który pobiera powiązanych danych Tak jak w przypadku eager ładowanie za pomocą oddzielne zapytania jawne ładowanie wyników w wielu zapytań wysyłanych do bazy danych. Różnica polega na tym, za pomocą jawne ładowanie kod określa właściwości nawigacji, aby go załadować. Entity Framework Core 1.1 umożliwia `Load` celu jawne ładowanie metodę. Na przykład:

  ![Przykład jawne ładowanie](read-related-data/_static/explicit-loading.png)

* Ładowanie z opóźnieniem. Podczas odczytywania jednostki powiązane dane nie są pobierane. Jednak użytkownik podejmie próbę dostępu do właściwości nawigacji po raz pierwszy wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Zapytanie jest wysyłane do bazy danych zawsze próbować pobierać dane z właściwości nawigacji, po raz pierwszy. Entity Framework Core 1.0 nie obsługuje ładowania z opóźnieniem.

### <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Jeśli znasz powiązane dane potrzebne dla każdej jednostki pobrać eager podczas ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysłanego do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania, dla każdej jednostki pobierane. Na przykład załóżmy, że każdy dział ma dziesięć Kursy pokrewne. Wczesne ładowanie wszystkich powiązanych danych mogłoby spowodować tylko zapytania do jednego (Połącz) i jeden obiegu do bazy danych. Oddzielnego zapytania dla kursy dla każdego działu mogłoby spowodować jedenaście rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwa dla wydajności, gdy opóźnienie jest wysokie.

Z drugiej strony w niektórych scenariuszach oddzielne zapytania jest bardziej wydajne. Wczesne ładowanie wszystkich powiązanych danych w jednym zapytaniu może spowodować bardzo złożone sprzężenia zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli potrzebujesz dostępu do właściwości nawigacji jednostki, tylko dla podzbioru zestaw jednostek, które one przetwarzanie, oddzielne zapytania może działać lepiej ponieważ wczesne ładowanie wszystkich elementów na początku spowoduje pobieranie większej ilości danych niż jest Ci potrzebne. Jeśli wydajność ma kluczowe znaczenie, najlepiej testowania wydajności w obu kierunkach, aby można było dokonanie najlepszego wyboru.

## <a name="create-a-courses-page"></a>Utwórz stronę kursów

Jednostki kurs zawiera właściwość nawigacji, która zawiera jednostkę działu działu, przypisana do kursu. Aby wyświetlić nazwę działu przypisany na liście kursy, musisz pobrać właściwości nazwy jednostki działu, która znajduje się w `Course.Department` właściwości nawigacji.

Utworzyć kontroler o nazwie CoursesController dla typu jednostki kurs przy użyciu tych samych opcji dla **kontroler MVC z widokami używający narzędzia Entity Framework** Generator szkieletu wykonanego wcześniej dla kontrolera studentów, jak pokazano w na poniższej ilustracji:

![Dodawanie kontrolera kursów](read-related-data/_static/add-courses-controller.png)

Otwórz *CoursesController.cs* i zbadaj `Index` metody. Automatyczne tworzenie szkieletów określono wczesne ładowanie dla `Department` właściwość nawigacji za pomocą `Include` metody.

Zastąp `Index` metoda następującym kodem, który używa bardziej odpowiednie nazwy dla `IQueryable` zwracającego kurs jednostki (`courses` zamiast `schoolContext`):

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Otwórz *Views/Courses/Index.cshtml* i Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Utworzony szkielet kodu zostały wprowadzone następujące zmiany:

* Zmienić nagłówek z indeksu do kursów.

* Dodano **numer** kolumna, która pokazuje `CourseID` wartości właściwości. Domyślnie klucze podstawowe nie są szkieletu, ponieważ zwykle są bez znaczenia dla użytkowników końcowych. Jednak w takim przypadku klucz podstawowy ma znaczenie i chcą ją wyświetlić.

* Zmienione **działu** kolumny, aby wyświetlić nazwę działu. Ten kod wyświetla `Name` właściwości jednostki działu, który jest ładowany do `Department` właściwość nawigacji:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę nazw działów.

![Kursy strony indeksu](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page"></a>Tworzenie strony instruktorów

W tej sekcji utworzysz kontroler i widok dla jednostki przez instruktorów, aby wyświetlić stronę Instruktorzy:

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

* Lista Instruktorzy wyświetli powiązane dane z jednostki OfficeAssignment. Jednostki przez instruktorów i OfficeAssignment znajdują się w relacji jeden do zero lub jeden. Wczesne ładowanie użyjemy dla jednostek OfficeAssignment. Jak wyjaśniono wcześniej, wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy będziesz potrzebować powiązanych danych we wszystkich wierszach pobrane w tabeli podstawowej. W tym przypadku chcesz wyświetlić przypisania pakietu office dla wszystkich wyświetlanych instruktorów.

* Gdy użytkownik wybierze pod kierunkiem instruktora, są wyświetlane z powiązanymi obiektami kursu. Jednostki przez instruktorów i kurs znajdują się w relacji wiele do wielu. Wczesne ładowanie służący do celów kursu jednostek i ich powiązanych jednostek działu. W tym przypadku oddzielne zapytania może być bardziej efektywne, ponieważ należy kursy tylko dla wybranych przez instruktorów. Jednak w tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w ramach jednostek, które znajdują się w oknie właściwości nawigacji.

* Gdy użytkownik wybierze kurs, dane związane z zestawu jednostek rejestracji jest wyświetlany. Jednostki kurs i rejestracji znajdują się w relacji jeden do wielu. Oddzielne zapytania służący do celów rejestracji jednostek i ich powiązanych jednostek dla uczniów.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Tworzenie modelu widoku dla widoku indeksu przez instruktorów

Na stronie Instruktorzy wyświetlane dane z trzech różnych tabelach. W związku z tym utworzysz zawiera trzy właściwości zawierający dane dla jednej z tabel, model widoku.

W *SchoolViewModels* folderze utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Tworzenie widoków i kontrolerów przez instruktorów

Utworzyć kontroler Instruktorzy z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:

![Dodawanie kontrolera instruktorów](read-related-data/_static/add-instructors-controller.png)

Otwórz *InstructorsController.cs* i Dodaj nazwę za pomocą instrukcji dla przestrzeni nazw modele widoków:

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Zastąp metodę indeksu następujący kod, aby nie wczesne ładowanie powiązanych danych i umieść ją w modelu widoku.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Metoda akceptuje dane opcjonalne trasy (`id`) i parametr ciągu zapytania (`courseID`), podaj wartości identyfikator wybranego przez instruktorów i wybranych kursów. Parametry są dostarczane przez **wybierz** hiperlinków na stronie.

Kod rozpoczyna się od tworzenia wystąpienia obiektu modelu widoku i umieszczenie w nim listy instruktorów. Kod ten określa wczesne ładowanie dla `Instructor.OfficeAssignment` i `Instructor.CourseAssignments` właściwości nawigacji. W ramach `CourseAssignments` właściwości `Course` właściwość jest załadowany oraz w ramach, `Enrollments` i `Department` właściwości są załadowane, a w ramach każdej `Enrollment` jednostki `Student` właściwość jest ładowany.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Ponieważ widoku zawsze wymaga jednostki OfficeAssignment, jest bardziej wydajne, można pobrać, w tym samym zapytaniu. Kurs jednostki są wymagane, gdy pod kierunkiem instruktora jest zaznaczona na stronie sieci web, więc pojedynczego zapytania jest lepsze niż wielu zapytań, tylko wtedy, gdy ta strona jest wyświetlana częściej kurs wybrana niż bez.

Kod jest powtarzany `CourseAssignments` i `Course` ponieważ potrzebne dwie właściwości z `Course`. Pierwszy ciąg `ThenInclude` wywołuje pobiera `CourseAssignment.Course`, `Course.Enrollments`, i `Enrollment.Student`.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

W tym momencie w kodzie inny `ThenInclude` byłoby dla właściwości nawigacji `Student`, który nie jest potrzebny. Ale wywołanie `Include` zaczyna się od ciągu `Instructor` właściwości, dzięki czemu będą musiały przejść przez łańcuch ponownie, określając ten czas `Course.Department` zamiast `Course.Enrollments`.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Poniższy kod wykonuje, gdy wybrano pod kierunkiem instruktora. Wybrane przez instruktorów są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwość jest następnie ładowany z jednostkami kurs z tym instruktora `CourseAssignments` właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko do jednej przez instruktorów jednostki zwracanego. `Single` Metoda konwertuje kolekcję na pojedynczej jednostki przez instruktorów, która zapewnia dostęp do tej jednostki `CourseAssignments` właściwości. `CourseAssignments` Właściwość zawiera `CourseAssignment` jednostek, z których mają tylko powiązane `Course` jednostek.

Możesz użyć `Single` metody w kolekcji, gdy wiadomo, Kolekcja będzie mieć tylko jeden element. Pojedyncza metoda zgłasza wyjątek, jeśli kolekcja wymagał jest pusta lub ma więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku) Jeśli kolekcja jest pusta. Jednak w takim przypadku, będzie nadal prowadzić do wyjątku (z próby znalezienia `Courses` właściwość odwołanie o wartości null), a komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Podczas wywoływania `Single` metody, można również przekazać kiedy warunek, zamiast wywoływać metodę `Where` metoda oddzielnie:

```csharp
.Single(i => i.ID == id.Value)
```

Zamiast:

```csharp
.Where(i => i.ID == id.Value).Single()
```

Następnie jeśli kurs został wybrany, wybranych kursów jest pobierana z Lista kursów model widoku. Następnie Wyświetl modelu `Enrollments` właściwość jest ładowany z jednostkami rejestracji z danego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Zmodyfikuj widok indeksu przez instruktorów

W *Views/Instructors/Index.cshtml*, Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Następujące zmiany wprowadzone do istniejącego kodu:

* Klasa modelu, aby zmienić `InstructorIndexData`.

* Zmienić tytuł strony z **indeksu** do **Instruktorzy**.

* Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacja jeden do zero lub jeden, nie może być powiązana jednostka OfficeAssignment.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodano **kursów** kolumnę wyświetlającą kursy prowadzone przez instruktora każdego. Zobacz [jawne przejście wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji na temat tej składni razor.

* Dodano kod, w której są dodawane dynamicznie `class="success"` do `tr` elementu wybranego przez instruktorów. To ustawienie jako kolor tła wybranego wiersza przy użyciu ładowania klasy.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze etykietą **wybierz** bezpośrednio przed innych linków w każdym wierszu, co powoduje, że identyfikator wybranego przez instruktorów do wysłania do `Index` metody.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz **Instruktorzy** kartę. Strony wyświetla Właściwość Location elementu komórki pustej tabeli i powiązanymi jednostkami OfficeAssignment po nie powiązanej jednostki OfficeAssignment.

![Strona indeksu instruktorów, którą nic nie zaznaczone](read-related-data/_static/instructors-index-no-selection.png)

W *Views/Instructors/Index.cshtml* pliku po zamykającym table — element (na końcu pliku), Dodaj następujący kod. Ten kod wyświetla listę kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także **wybierz** hiperłącze, które wysyła identyfikator wybranego kurs, aby `Index` metody akcji.

Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora. Spowoduje to wyświetlenie siatce, która wyświetla kursy przypisane do wybranego przez instruktorów i każdego kursu możesz zobaczyć nazwę tego działu przypisane.

![Wybrane przez instruktorów indeks strony instruktorów](read-related-data/_static/instructors-index-instructor-selected.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Spowoduje to wyświetlenie listy uczniów, którzy są rejestrowane kursu, po wybraniu danego kursu.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Ten kod odczytuje właściwość rejestracji modelu widoku, aby wyświetlić listę uczniów zarejestrowane w ramach tego kursu.

Ponownie Odśwież stronę, a następnie wybierz pozycję pod kierunkiem instruktora. Następnie wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instruktorzy indeks strony instruktora oraz przypisane kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="about-explicit-loading"></a>Temat jawne ładowanie

Po pobraniu listy Instruktorzy w *InstructorsController.cs*, określony wczesne ładowanie dla `CourseAssignments` właściwości nawigacji.

Załóżmy, że oczekiwany użytkownikom tylko rzadko mają być wyświetlane rejestracje w wybrany przez instruktorów i kursów. W takim przypadku można załadować danych rejestracji, tylko wtedy, gdy jest żądany. Aby zobaczyć przykład sposobu wykonania jawne ładowanie, Zastąp `Index` metoda poniższym kodem, co spowoduje usunięcie wczesne ładowanie na potrzeby rejestracji i ładuje jawnie tej właściwości. Zmiany kodu są wyróżnione.

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Nowy kod spada *ThenInclude* metoda wywołuje danych rejestracji z kodu, która pobiera jednostki instruktora. Jeśli wybrano przez instruktorów i kursów, wyróżniony kod pobiera jednostki rejestracji dla wybranych kursów i uczniów jednostki dla każdej rejestracji.

Uruchom aplikację, przejdź do strony indeksu Instruktorzy teraz i będzie żadnej widocznej różnicy w wyświetlanych na stronie, mimo że zostało zmienione, jak dane są pobierane.

## <a name="get-the-code"></a>Pobierz kod

[Pobieranie i wyświetlanie ukończonej aplikacji.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Dowiedzieliśmy się, jak załadować dane dotyczące
> * Utworzona strona kursów
> * Utworzona strona instruktorów
> * Przedstawia informacje na temat jawne ładowanie

Przejdź do następnego samouczka, aby dowiedzieć się, jak aktualizowanie powiązanych danych.

> [!div class="nextstepaction"]
> [Aktualizowanie powiązanych danych](update-related-data.md)
