---
title: "Platformy ASP.NET Core MVC podstawowych EF - odczytanie danych powiązanych — 6 10"
author: tdykstra
description: "W tym samouczku będziesz odczytu i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji."
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 58b05587458aacad1a633a04f0359a4d2a3605a3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Odczytywanie powiązane dane - Core EF z samouczek platformy ASP.NET Core MVC (6 10)

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W samouczku poprzedniej została ukończona modelu danych służbowych. W tym samouczku możesz przeczytać i wyświetlanie powiązanych danych — to znaczy dane programu Entity Framework wczytywane właściwości nawigacji.

Na poniższych ilustracjach przedstawiono stron, że będzie działać z.

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Wczesny jawne i opóźnionego ładowania powiązanych danych

Istnieje kilka sposobów oprogramowania obiektów relacyjnych mapowania (ORM), takie jak Entity Framework można załadować powiązanych danych do właściwości nawigacji jednostki:

* Ładowanie wczesny. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne. Określ wczesny ładowania w Entity Framework Core za pomocą `Include` i `ThenInclude` metody.

  ![Przykład wczesny ładowania](read-related-data/_static/eager-loading.png)

  Możesz pobrać niektóre dane w oddzielne zapytania, a EF "rozwiązuje" właściwości nawigacji.  Oznacza to EF automatycznie doda oddzielnie pobrane jednostek, w których one należą do właściwości nawigacji jednostek wcześniej pobrane. Dla zapytania, które pobiera dane powiązane, można użyć `Load` metody zamiast metody, która zwraca listy lub obiektu, takie jak `ToList` lub `Single`.

  ![Przykład oddzielne zapytania](read-related-data/_static/separate-queries.png)

* Jawne ładowania. Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Możesz pisania kodu, który pobiera dane powiązane, jeśli jest to potrzebne. Tak jak w przypadku eager ładowania z oddzielne zapytania jawne, ładowanie wyników w kwerendach wielu wysyłane do bazy danych. Różnica polega na z jawnego ładowania kodu określa właściwości nawigacji do załadowania. W Entity Framework Core 1.1 można użyć `Load` metody w celu jawnego ładowania. Na przykład:

  ![Przykład jawnego ładowania](read-related-data/_static/explicit-loading.png)

* Powolne ładowanie. Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Jednak podczas pierwszej próby dostępu do właściwości nawigacji wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Zapytanie jest wysyłane do bazy danych zawsze próbować pobierać dane z właściwości nawigacji, po raz pierwszy. Entity Framework Core 1.0 nie obsługuje ładowania opóźnieniem.

### <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Jeśli znasz potrzebne dane dotyczące dla każdej jednostki pobrać wczesny ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysyłane do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania dla każdej jednostki pobrać. Na przykład załóżmy, że każdy dział ma dziesięć Kursy pokrewne. Ładowanie wczesny wszystkich powiązanych danych spowoduje tylko zapytania jednym (połączenie) i jeden obiegu do bazy danych. Oddzielne zapytania kursów dla każdego działu spowoduje 11 rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwe dla wydajności, gdy dużych opóźnieniach.

Z drugiej strony w niektórych scenariuszach oddzielne zapytania jest bardziej wydajny. Ładowanie wczesny wszystkich powiązanych danych w jednym zapytaniu może spowodować sprzężenia bardzo skomplikowane, zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli potrzebujesz dostępu do właściwości nawigacji jednostki tylko przez podzbiór zestawu jednostek, który jest przetwarzanie, oddzielne zapytania mogą działać lepiej ponieważ wczesny ładowania wszystkich elementów na początku czy pobrać więcej danych niż trzeba. Jeśli wydajność jest szczególnie ważne, najlepiej testowanie wydajności w obu kierunkach, aby ustawić najlepszym rozwiązaniem.

## <a name="create-a-courses-page-that-displays-department-name"></a>Utwórz stronę kursy Wyświetla nazwę działu

Jednostka kursu obejmuje zawierającym jednostkę działu działu kursu przypisane do właściwości nawigacji. Aby wyświetlić nazwę działu przypisane na liście kursów, należy uzyskać właściwość Name z jednostki działu, który znajduje się w `Course.Department` właściwości nawigacji.

Tworzenie kontrolera o nazwie CoursesController dla porach typu jednostki, przy użyciu tych samych opcji dla **kontroler MVC z widokami używający narzędzia Entity Framework** tworzenia szkieletu, która została wcześniej kontrolera studentów, jak pokazano w poniższej ilustracji:

![Dodawanie kontrolera kursy](read-related-data/_static/add-courses-controller.png)

Otwórz *CoursesController.cs* i sprawdź, czy `Index` metody. Automatyczne szkieletów określono wczesny ładowania dla `Department` właściwość nawigacji za pomocą `Include` metody.

Zastąp `Index` metodę z następującym kodem, który używa bardziej odpowiednie nazwy dla `IQueryable` zwracającą kursu jednostek (`courses` zamiast `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Otwórz *Views/Courses/Index.cshtml* i Zastąp kod szablonu z następującym kodem. Zmiany są wyróżnione:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Wprowadzono następujące zmiany do szkieletu kodu:

* Zmieniony nagłówek od indeksu do kursów.

* Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości. Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych. Jednak w takim przypadku klucza podstawowego ma znaczenie i chcesz je wyświetlić.

* Zmienione **działu** kolumny, aby wyświetlić nazwę działu. Wyświetla kod `Name` właściwości jednostki działu, który jest ładowany do `Department` właściwość nawigacji:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Uruchom aplikację i wybierz **kursów** kartę, aby wyświetlić listę z nazwami działu.

![Strona indeksu kursy](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów kursów i rejestracji

W tej sekcji utworzysz kontrolera i widoku dla obiekt instruktora w celu wyświetlenia strony instruktorów:

![Strona indeksu instruktorów](read-related-data/_static/instructors-index.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

* Na liście instruktorów zostaną wyświetlone dane powiązane z OfficeAssignment jednostki. Jednostki instruktora i OfficeAssignment znajdują się w relacji jeden do zero lub jeden. Ładowanie wczesny będzie używany dla jednostek OfficeAssignment. Zgodnie z opisem, wczesny ładowania jest zazwyczaj bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy w tabeli podstawowej. W takim przypadku mają być wyświetlane przypisań pakietu office dla wszystkich wyświetlanych instruktorów.

* Po wybraniu instruktora powiązanych jednostek kursu są wyświetlane. Jednostki instruktora i kursu znajdują się w relacji wiele do wielu. Użyjesz wczesny ładowanie obiektów kursu i ich powiązanych jednostek działu. W takim przypadku oddzielne zapytania może być bardziej wydajne, ponieważ należy kursy tylko dla wybranego instruktora. Jednak ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w ramach jednostkami, którymi na właściwości nawigacji.

* Gdy użytkownik wybierze kursu, są wyświetlane powiązane dane z zestawu jednostek rejestracji. Jednostki przebiegu i rejestracji są w relacji jeden do wielu. Użyjesz oddzielne zapytania rejestracji obiektów i ich powiązanych jednostek studenta.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz model widoku dla widoku indeksu instruktora

Na stronie instruktorów znajdują się dane z trzech różnych tabel. W związku z tym utworzysz modelu widoku, który zawiera trzy właściwości, zawierający dane z tabel.

W *SchoolViewModels* folderu, Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Tworzenie kontrolera instruktora i widoków

Tworzenie kontrolera instruktorów z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:

![Dodawanie kontrolera instruktorów](read-related-data/_static/add-instructors-controller.png)

Otwórz *InstructorsController.cs* i dodać za pomocą instrukcji dla przestrzeni nazw ViewModels:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Zastąp następujący kod do czy ładowanie wczesny powiązanych danych i umieść go w modelu widoku metody indeksu.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Metoda akceptuje dane trasy opcjonalne (`id`) i parametr ciągu zapytania (`courseID`) umożliwiające wartości Identyfikatora wybranym instruktorze i wybranego kursu. Parametry są dostarczane przez **wybierz** hiperłącza, na stronie.

Kod rozpoczyna się przez utworzenie wystąpienia modelu widoku i umieszczenie w nim listy instruktorów. Kod określa wczesny ładowania dla `Instructor.OfficeAssignment` i `Instructor.CourseAssignments` właściwości nawigacji. W ramach `CourseAssignments` właściwość `Course` właściwość jest załadowany oraz w ramach, `Enrollments` i `Department` właściwości są załadowane, a w ramach każdej `Enrollment` jednostki `Student` właściwości jest załadowany.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Ponieważ widok wymaga zawsze OfficeAssignment jednostki, jest bardziej wydajne, można pobrać który w jednym zapytaniu. Jednostek kursu są wymagane w przypadku instruktora jest zaznaczona na stronie sieci web, więc pojedynczego zapytania jest lepszym rozwiązaniem niż wielu zapytań tylko wtedy, gdy strona jest wyświetlana częściej z kursu wybrane niż bez.

Kod jest powtarzany `CourseAssignments` i `Course` ponieważ potrzebne dwie właściwości z `Course`. Pierwszy ciąg `ThenInclude` wywołuje pobiera `CourseAssignment.Course`, `Course.Enrollments`, i `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

W tym momencie w kodzie innego `ThenInclude` do właściwości nawigacji `Student`, który nie jest potrzebny. Ale wywoływania `Include` rozpoczyna się od za pośrednictwem `Instructor` właściwości, więc musisz przejść przez łańcuch ponownie, ten czas, określając `Course.Department` zamiast `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Poniższy kod wykonuje się, gdy wybrano instruktora. Wybranym instruktorze są pobierane z listy instruktorów w modelu widoku. Model widoku `Courses` właściwości jest następnie ładowany z kursu jednostek z tego instruktora `CourseAssignments` właściwości nawigacji.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko do jednej instruktora jednostki zostały zwrócone. `Single` Metoda konwertuje kolekcję do pojedynczej jednostki instruktora, która umożliwia dostęp do tej jednostki `CourseAssignments` właściwości. `CourseAssignments` Właściwość zawiera `CourseAssignment` jednostek, z których chcesz tylko powiązane `Course` jednostek.

Możesz użyć `Single` metoda w kolekcji, gdy wiesz, Kolekcja będzie mieć tylko jeden element. Pojedyncza metoda zgłasza wyjątek, jeśli kolekcja przekazywania jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest `SingleOrDefault`, która zwraca wartość domyślną (wartość null, w tym przypadku), jeśli kolekcja jest pusta. Jednak w takim przypadku który nadal spowoduje powstanie wyjątku (z próby znalezienia `Courses` właściwości na odwołanie o wartości null), a komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Podczas wywoływania `Single` metody, można również przekazać Where warunku zamiast wywoływać metodę `Where` metody oddzielnie:

```csharp
.Single(i => i.ID == id.Value)
```

Zamiast:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Obok Jeśli wybrano kursu, wybranego kursu są pobierane z listy kursy w modelu widoku. Następnie widok modelu `Enrollments` właściwości jest załadowana rejestracji jednostek z tego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Zmodyfikuj widok indeksu instruktora

W *Views/Instructors/Index.cshtml*, Zastąp kod szablonu z następującym kodem. Zmiany zostały wyróżnione.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Następujące zmiany wprowadzone do istniejącego kodu:

* Klasa modelu, aby zmienić `InstructorIndexData`.

* Zmienić tytuł strony z **indeksu** do **instruktorów**.

* Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacji jeden do zero lub jeden, być może obiekt pokrewny OfficeAssignment.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Dodaje **kursów** kolumnę wyświetlającą kursy nauczanych przy każdym instruktora. Zobacz [jawne przejścia wiersza z `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) więcej informacji o tej składni razor.

* Dodano kod, który dynamicznie dodaje `class="success"` do `tr` elementu wybranego instruktora. To ustawia kolor tła dla wybranego wiersza za pomocą klasy ładowania początkowego.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Dodano nowe hiperłącze etykietą **wybierz** bezpośrednio przed innymi łączy w każdym wierszu, co powoduje, że identyfikator wybranego instruktora do wysłania do `Index` metody.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Uruchom aplikację i wybierz **instruktorów** kartę. Zostaje wyświetlona strona właściwości Location powiązanych jednostek OfficeAssignment i puste komórki po nie powiązanych jednostek OfficeAssignment.

![Strona indeksu instruktorów nic nie wybrane](read-related-data/_static/instructors-index-no-selection.png)

W *Views/Instructors/Index.cshtml* plik, po zamknięcia tabeli elementu (na końcu pliku), Dodaj następujący kod. Ten kod wyświetla listę kursów związane z instruktora po wybraniu instruktora.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także **wybierz** hiperłącze, które wysyła identyfikator wybranego kursu do `Index` metody akcji.

Odśwież stronę i wybierz instruktora. Spowoduje to wyświetlenie Siatka wyświetla kursy przypisane do wybranych instruktora i dla każdego kursu wyświetlona nazwa przypisanej działu.

![Instruktora strony indeksu instruktorów wybrane](read-related-data/_static/instructors-index-instructor-selected.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Lista zawiera studentów, którzy są zarejestrowane w toku w przypadku wybrania tego kursu.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Ten kod odczytuje właściwości rejestracji modelu widoku, aby wyświetlić listę studentów zarejestrowane w trakcie.

Ponownie Odśwież stronę i wybierz instruktora. Następnie wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instruktora strony indeksu instruktorów i kursów wybranych](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Ładowanie jawne

Po pobraniu listy w instruktorów *InstructorsController.cs*, określony wczesny ładowania dla `CourseAssignments` właściwości nawigacji.

Załóżmy, że oczekiwany użytkownikom rzadko mają być wyświetlane rejestracji w wybranym instruktorze oraz przebiegu. W takim przypadku można załadować danych rejestrowania, tylko wtedy, gdy żądanie. Aby wyświetlić przykład sposobu wykonania jawnego ładowania, Zastąp `Index` metodę z następującym kodem, który usuwa wczesny ładowania dla rejestracji i ładuje jawnie tej właściwości. Zmiany kodu zostały wyróżnione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Nowy kod porzuca *ThenInclude* metoda wywołuje danych rejestracji z kod, który pobiera instruktora jednostki. Jeśli nie wybrano instruktora oraz przebiegu, wyróżniony kod pobiera jednostek rejestracji dla porach wybranych i uczniów jednostki dla każdej rejestracji.

Uruchom aplikację, przejdź do strony indeksu instruktorów teraz i będzie żadnej widocznej różnicy w wyświetlanych na stronie, mimo że zostały zmienione, jak dane są pobierane.

## <a name="summary"></a>Podsumowanie

Teraz wczesny ładowanie z jednego zapytania i używane z wieloma zapytaniami do wczytania powiązanych danych do właściwości nawigacji. W następnym samouczku nauczysz się, jak zaktualizować powiązanych danych.

>[!div class="step-by-step"]
>[Poprzednie](complex-data-model.md)
>[dalej](update-related-data.md)  
