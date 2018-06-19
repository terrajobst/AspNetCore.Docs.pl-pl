---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Odczytywanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 06784d8b610856e71eae78b0db2d0253faedb955
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875331"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Odczytywanie powiązane dane z programu Entity Framework w aplikacji platformy ASP.NET MVC
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednich instrukcji została ukończona modelu danych służbowych. W tym samouczku będziesz odczytu i wyświetlanie powiązanych danych, oznacza to, że dane programu Entity Framework ładuje do właściwości nawigacji.

Na poniższych ilustracjach przedstawiono stron, że będzie działać z.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Ładowanie opóźnieniem wczesny i jawne powiązanych danych

Istnieje kilka sposobów programu Entity Framework może ładować powiązanych danych do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Jednak podczas pierwszej próby dostępu do właściwości nawigacji wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Powoduje to wiele zapytań wysłanych do bazy danych — jeden dla sam podmiot i jeden musi zostać pobrany za każdym razem powiązane dane dla tej jednostki. `DbContext` Klasa umożliwia opóźnionego ładowania domyślnie. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Ładowanie wczesny*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne. Określ wczesny ładowania przy użyciu `Include` metody.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowania*. Jest to podobne do opóźnionego ładowania, z wyjątkiem tego, że jawnie pobrać powiązanych danych w kodzie; go nie jest realizowane automatycznie podczas dostępu do właściwości nawigacji. Ręcznie załadować dane dotyczące pobierając wpis Menedżera stanu obiektu dla jednostki i wywoływania [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) metody dla kolekcji lub [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) metodę dla właściwości, które zawierają pojedynczy element. (W poniższym przykładzie, aby załadować właściwość nawigacji administratora, należy zastąpić `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.) Zazwyczaj można użyć tylko wtedy, gdy została włączona ładowania poza opóźnionego jawnego ładowania.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, opóźnionego ładowania i jawnego ładowania również zarówno nazywamy *odroczonego ładowania*.

### <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Jeśli znasz potrzebne dane dotyczące dla każdej jednostki pobrać wczesny ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysyłane do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania dla każdej jednostki pobrać. Załóżmy na przykład, w powyższych przykładach, że każdy dział ma dziesięć Kursy pokrewne. W przykładzie wczesny ładowania spowoduje tylko zapytania jednym (połączenie) i jeden obiegu do bazy danych. Powolne ładowanie i przykłady jawnego ładowania zarówno spowoduje 11 zapytania i 11 rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwe dla wydajności, gdy dużych opóźnieniach.

Z drugiej strony w niektórych scenariuszach opóźnionego ładowania jest bardziej wydajny. Ładowanie wczesny może spowodować sprzężenia bardzo skomplikowane, zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli chcesz uzyskać dostęp do właściwości nawigacji jednostki tylko przez podzbiór zestawu jednostek w przypadku przetwarzania, opóźnionego ładowania mogą działać lepiej, ponieważ ładowanie wczesny czy pobrać więcej danych niż trzeba. Jeśli wydajność jest szczególnie ważne, najlepiej testowanie wydajności w obu kierunkach, aby ustawić najlepszym rozwiązaniem.

Powolne ładowanie można zamaskować kodu, która powoduje występowanie problemów z wydajnością. Na przykład kodu, który nie określa wczesny lub jawnego ładowania, ale przetwarza dużą liczbę jednostek i używa kilku właściwości nawigacji w każdej iteracji może być bardzo mało wydajne (ze względu na wiele rund do bazy danych). Aplikacja, która wykonuje również Programowanie przy użyciu na lokalnym programem SQL server mogą wystąpić problemy z wydajnością po przeniesieniu do bazy danych SQL Azure z powodu większe opóźnienia i opóźnionego ładowania. Profilowanie zapytania bazy danych z realistyczne testu obciążenia pomoże określić, czy ładowanie opóźnieniem jest odpowiednia. Aby uzyskać więcej informacji, zobacz [Demystifying Entity Framework strategii: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [przy użyciu programu Entity Framework w celu zmniejszenia opóźnienia sieci SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Wyłączenie ładowania opóźnionego przed serializacji

Pozostawienie ładowania opóźnionego włączone podczas serializacji, może kończyć się badania znacznie więcej danych niż zamierzony. Serializacja zazwyczaj polega na uzyskiwanie dostępu do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala opóźnionego ładowania i podmioty załadować opóźnieniem są serializowane. Następnie procesu serializacji uzyskuje dostęp do każdej właściwości jednostek opóźnieniem załadować może powodować więcej opóźnionego ładowania i serializacji. Aby uniknąć tego łańcuchową run-away, Włącz opóźnionego ładowania wyłączone przed serializować jednostki.

Serializacja może również być skomplikowany klasy serwera proxy, które korzysta z programu Entity Framework, zgodnie z objaśnieniem w [samouczek zaawansowane scenariusze](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Jeden sposób na uniknięcie problemów serializacji jest do serializacji obiektów transfer danych (DTOs) zamiast obiektów jednostek, jak pokazano w [przy użyciu interfejsu API sieci Web z programu Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) samouczka.

Jeśli nie używasz DTOs, można wyłączyć ładowania opóźnionego i uniknąć problemów z serwera proxy przez [wyłączenie serwera proxy tworzenia](https://msdn.microsoft.com/data/jj592886.aspx).

Poniżej przedstawiono niektóre inne [sposoby wyłączenia opóźnionego ładowania](https://msdn.microsoft.com/data/jj574232):

- Dla właściwości nawigacji określonych, Pomiń `virtual` — słowo kluczowe w deklaracji właściwości.
- Dla wszystkich właściwości nawigacji, ustaw `LazyLoadingEnabled` do `false`, umieść następujący kod w konstruktorze klasy kontekstu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Utwórz nazwę tego działu wyświetla stronę kursy

`Course` Jednostki zawiera właściwość nawigacji, który zawiera `Department` jednostki działu przypisana do ciągu. Aby wyświetlić nazwę działu przypisane na liście kursów, należy uzyskać `Name` właściwość z `Department` jednostki, która znajduje się w `Course.Department` właściwości nawigacji.

Tworzenie kontrolera o nazwie `CourseController` (nie CoursesController) dla `Course` typu jednostki, przy użyciu tych samych opcji dla **kontroler MVC 5 z widokami używający narzędzia Entity Framework** tworzenia szkieletu, która została wcześniej `Student` kontrolera, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Otwórz *Controllers\CourseController.cs* i przyjrzyj się `Index` metody:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatyczne szkieletów określono wczesny ładowania dla `Department` właściwość nawigacji za pomocą `Include` metody.

Otwórz *Views\Course\Index.cshtml* i Zastąp kod szablonu z następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Wprowadzono następujące zmiany do szkieletu kodu:

- Zmieniony nagłówek z **indeksu** do **kursów**.
- Dodaje **numer** kolumny, która zawiera `CourseID` wartości właściwości. Domyślnie nie są szkieletu kluczy podstawowych, ponieważ zwykle są bezużyteczne dla użytkowników końcowych. Jednak w takim przypadku klucza podstawowego ma znaczenie i chcesz je wyświetlić.
- Przenieść **działu** kolumnę z prawej strony i zmienić jej nagłówek. Tworzenia szkieletu postanowiono poprawnie wyświetlić `Name` właściwość z `Department` jednostki, ale tutaj na stronie kursu nagłówek kolumny powinna być **działu** zamiast **nazwa**.

Zauważ, że w kolumnie Dział szkieletu kodu wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Uruchom strony (wybierz **kursów** kartę na stronie głównej University firmy Contoso) aby wyświetlić listę z nazwami działu.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów kursów i rejestracji

W tej sekcji zostanie utworzona kontrolera i wyświetlić dla `Instructor` jednostki, aby wyświetlić stronę instruktorów:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

- Lista instruktorów powiązane dane z `OfficeAssignment` jednostki. `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Użyjesz wczesny ładowania dla `OfficeAssignment` jednostek. Zgodnie z opisem, wczesny ładowania jest zazwyczaj bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy w tabeli podstawowej. W takim przypadku mają być wyświetlane przypisań pakietu office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze instruktora związane z `Course` wyświetlania obiektów. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Użyjesz wczesny ładowania dla `Course` jednostek i ich pokrewnych `Department` jednostek. W takim przypadku opóźnionego ładowania może być bardziej wydajne, ponieważ należy kursy tylko dla wybranego instruktora. Jednak ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w ramach jednostkami, którymi na właściwości nawigacji.
- Po wybraniu kursu powiązane dane z `Enrollments` wyświetlany jest zestaw jednostek. `Course` i `Enrollment` jednostki są w relacji jeden do wielu. Należy dodać jawnego ładowania dla `Enrollment` jednostek i ich pokrewnych `Student` jednostek. (Podczas jawnego ładowania jest niezbędne, ponieważ opóźnionego ładowania jest włączona, ale oznacza to, jak to zrobić podczas jawnego ładowania).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz Model widoku dla widoku indeksu instruktora

Na stronie instruktorów znajdują się trzech różnych tabel. W związku z tym utworzysz modelu widoku, który zawiera trzy właściwości, zawierający dane z tabel.

W *ViewModels* folderu, Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Tworzenie kontrolera instruktora i widoków

Utwórz `InstructorController` (nie InstructorsController) kontroler z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Otwórz *Controllers\InstructorController.cs* i Dodaj `using` instrukcji dla `ViewModels` przestrzeni nazw:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Kod z utworzonym szkieletem w `Index` metody określa wczesny ładowania tylko w przypadku `OfficeAssignment` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Zastąp `Index` metody z następujący kod, aby załadować dodatkowe dane dotyczące i umieszcza je w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Metoda akceptuje dane trasy opcjonalne (`id`) i parametr ciągu zapytania (`courseID`) podaj wartości Identyfikatora wybranym instruktorze i wybranych przebieg i przekazuje wszystkie dane wymagane do widoku. Parametry są dostarczane przez **wybierz** hiperłącza, na stronie.

Kod rozpoczyna się przez utworzenie wystąpienia modelu widoku i umieszczenie w nim listy instruktorów. Kod określa wczesny ładowania dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Drugi `Include` metody ładuje kursy i dla każdego kursu, który jest ładowany jest wczesny ładowania `Course.Department` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Jak wspomniano wcześniej, ładowanie wczesny nie jest wymagany, ale odbywa się w celu zwiększenia wydajności. Ponieważ widok zawsze wymaga `OfficeAssignment` jednostki, jest bardziej wydajne, można pobrać który w jednym zapytaniu. `Course` jednostki są wymagane w przypadku instruktora jest zaznaczona na stronie sieci web, ładowanie wczesny jest lepszym rozwiązaniem niż opóźnionego ładowania tylko wtedy, gdy strona jest wyświetlana częściej z kursu wybrane niż bez.

Jeśli wybrano Identyfikator instruktora, wybranym instruktorze są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwości jest następnie ładowany z `Course` jednostek z tego instruktora `Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko jeden `Instructor` są zwracane jednostki. `Single` Metoda konwertuje kolekcję do postaci jednej `Instructor` jednostki, która umożliwia dostęp do tej jednostki `Courses` właściwości.

Możesz użyć [pojedynczego](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metoda w kolekcji, gdy wiesz, Kolekcja będzie mieć tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja przekazywania jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w takim przypadku), jeśli kolekcja jest pusta. Jednak w takim przypadku który nadal spowoduje powstanie wyjątku (z próby znalezienia `Courses` właściwość `null` odwołania), oraz komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Podczas wywoływania `Single` metody, można również przekazać `Where` warunku zamiast wywoływać metodę `Where` metody oddzielnie:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Obok Jeśli wybrano kursu, wybranego kursu są pobierane z listy kursy w modelu widoku. Następnie widok modelu `Enrollments` właściwości jest ładowany z `Enrollment` jednostek z tego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Zmodyfikuj widok indeksu instruktora

W *Views\Instructor\Index.cshtml*, Zastąp kod szablonu z następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Następujące zmiany wprowadzone do istniejącego kodu:

- Klasa modelu, aby zmienić `InstructorIndexData`.
- Zmienić tytuł strony z **indeksu** do **instruktorów**.
- Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie jest zerowa. (Ponieważ jest to relacji jeden do zero lub jeden, może nie być powiązanego `OfficeAssignment` jednostki.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Dodano kod, który będzie dynamicznie dodać `class="success"` do `tr` elementu wybranego instruktora. To ustawia kolor tła dla wybranego wiersza za pomocą [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) klasy. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Dodano nową `ActionLink` etykietą **wybierz** bezpośrednio przed innymi łączy w każdym wierszu, co powoduje, że identyfikator wybranego instruktora do wysłania do `Index` metody.

Uruchom aplikację i wybierz **instruktorów** kartę. Na stronie są wyświetlane `Location` powiązanych właściwości `OfficeAssignment` jednostek i pusta tabela komórki, gdy istnieje nr związane z `OfficeAssignment` jednostki.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

W *Views\Instructor\Index.cshtml* plików po upływie `table` elementu (na końcu pliku), Dodaj następujący kod. Ten kod wyświetla listę kursów związane z instruktora po wybraniu instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także `Select` hiperłącze, które wysyła identyfikator wybranego kursu do `Index` metody akcji.

Uruchom strony i wybierz instruktora. Spowoduje to wyświetlenie Siatka wyświetla kursy przypisane do wybranych instruktora i dla każdego kursu wyświetlona nazwa przypisanej działu.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Lista zawiera studentów, którzy są zarejestrowane w toku w przypadku wybrania tego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ten kod odczytuje `Enrollments` właściwości modelu widoku, aby wyświetlić listę studentów zarejestrowane w toku.

Uruchom strony i wybierz instruktora. Następnie wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Dodawanie jawnego ładowania

Otwórz *InstructorController.cs* i znajduje się w sposób `Index` metoda pobiera listę rejestracji dla porach wybranych:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Po pobraniu listy instruktorów określony wczesny ładowania dla `Courses` właściwość nawigacji i `Department` właściwości każdego kursu. Następnie możesz umieścić `Courses` kolekcji w modelu widoku, a teraz uzyskujesz dostęp do `Enrollments` właściwość nawigacji z jednego obiektu w tej kolekcji. Ponieważ nie określono wczesny ładowania dla `Course.Enrollments` właściwość nawigacji, w wyniku opóźnionego ładowania strony jest wyświetlane dane z tej właściwości.

Wyłączenie ładowania opóźnionego bez zmiany kodu w inny sposób `Enrollments` właściwość może mieć wartości null, niezależnie od tego, ile rejestracji kursu faktycznie ma. W takim przypadku można załadować `Enrollments` właściwość, należy określić wczesny ładowania lub jawnego ładowania. Już przedstawiono sposób wykonywania wczesny ładowania. Aby uzyskać przykład jawnego ładowania, Zastąp `Index` metodę z następującym kodem, które jawnie ładuje `Enrollments` właściwości. Wyróżniono kod został zmieniony.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Po pierwsze wybranego `Course` jednostki, nowy kod ładuje jawnie tego kursu `Enrollments` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Następnie ładuje jawnie każdego `Enrollment` jednostki użytkownika związane z `Student` jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Należy zauważyć, że używasz `Collection` metodę, aby załadować właściwość kolekcji, ale dla właściwości, który zawiera tylko jedną jednostkę, użyj `Reference` — metoda.

Teraz uruchom strony indeksu instruktora i zobaczysz różnicy w wyświetlanych na stronie, mimo że zostały zmienione, jak dane są pobierane.

## <a name="summary"></a>Podsumowanie

Teraz używano wszystkie trzy sposoby (opóźnieniem wczesny i jawne) ładowanie powiązanych danych do właściwości nawigacji. W następnym samouczku nauczysz się, jak zaktualizować powiązanych danych.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [dalej](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
