---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Odczytywanie danych powiązanych z platformą Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
ms.date: 11/07/2014
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4147feda2b78eefa2f5e280e587759585da738b3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839142"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Odczytywanie powiązanych danych, za pomocą programu Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednim samouczku można wykonać modelu danych służbowych. W tym samouczku będziesz odczytywać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, którą będziesz pracować.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Z opóźnieniem, Eager i jawne ładowanie powiązanych danych

Istnieje kilka sposobów, platformy Entity Framework można załadować powiązane dane do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Podczas odczytywania jednostki powiązane dane nie są pobierane. Jednak użytkownik podejmie próbę dostępu do właściwości nawigacji po raz pierwszy wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Skutkiem wielu zapytań wysyłanych do bazy danych — jedną dla sam podmiot, a co za każdym razem, powiązane dane dla jednostki musi zostać pobrany. `DbContext` Klasa umożliwia powolne ładowanie domyślnie. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Wczesne ładowanie*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne. Należy określić wczesne ładowanie za pomocą `Include` metody.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, chyba że jawnie pobrać powiązanych danych w kodzie nie będzie automatycznie gdy uzyskujesz dostęp do właściwości nawigacji. Ręczne ładowanie powiązanych danych uzyskując wpis Menedżera stanu obiektu do jednostki i wywoływania [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) metody dla kolekcji lub [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) metody, właściwości, które zawierają pojedynczy element. (W poniższym przykładzie, jeśli chcesz załadować właściwość nawigacji administratora, należy zamienić `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.) Zazwyczaj należy użyć jawne ładowanie tylko wtedy, gdy ta wiadomość została wysłana z opóźnieniem, ładowanie wyłączone.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, powolne ładowanie i jawne ładowanie są również zarówno określane jako *odroczone ładowanie*.

### <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności

Jeśli znasz powiązane dane potrzebne dla każdej jednostki pobrać eager podczas ładowania często oferuje najlepszą wydajność, ponieważ pojedynczego zapytania wysłanego do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania, dla każdej jednostki pobierane. W powyższych przykładach, załóżmy, że każdy dział ma dziesięć Kursy pokrewne. W przykładzie wczesne ładowanie spowoduje tylko zapytania do jednego (Połącz) i jeden obiegu do bazy danych. Powolne ładowanie i przykłady jawne ładowanie zarówno spowodowałaby jedenaście zapytania i jedenaście rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwa dla wydajności, gdy opóźnienie jest wysokie.

Z drugiej strony w niektórych scenariuszach powolne ładowanie jest bardziej wydajne. Wczesne ładowanie może powodować bardzo złożone sprzężenia zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli potrzebujesz uzyskać dostęp do właściwości nawigacji jednostki, tylko dla podzbioru zbiór jednostek jest przetwarzanie, powolne ładowanie może działać lepiej, ponieważ wczesne ładowanie może pobrać większej ilości danych niż jest Ci potrzebne. Jeśli wydajność ma kluczowe znaczenie, najlepiej testowania wydajności w obu kierunkach, aby można było dokonanie najlepszego wyboru.

Powolne ładowanie może maskować kod, który powoduje, że problemy z wydajnością. Na przykład kod, który nie określa eager lub jawny ładowania, ale przetwarza dużą liczbę jednostek i wykorzystuje kilka właściwości nawigacji w każdej iteracji może być nieefektywna (ze względu na wiele rund do bazy danych). Aplikacja, która wykonuje również Programowanie przy użyciu serwera SQL na lokalnych mogą wystąpić problemy z wydajnością po przeniesieniu do usługi Azure SQL Database ze względu na większe opóźnienia i ładowania z opóźnieniem. Profilowanie zapytania do bazy danych przy użyciu realistycznej testu obciążenia pomoże określić, czy powolne ładowanie jest odpowiednia. Aby uzyskać więcej informacji, zobacz [Demystifying strategie programu Entity Framework: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [używający narzędzia Entity Framework w celu zmniejszenia opóźnienia sieci na platformie Azure SQL](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Wyłączenie ładowania z opóźnieniem, przed serializacji

Pozostawienie powolne ładowanie włączone podczas serializacji, może zakończyć się znacznie większej ilości danych niż planowano zapytań. Serializacja zazwyczaj polega na dostęp do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala powolne ładowanie i podmioty załadowanych z opóźnieniem są serializowane. Następnie procesem serializacji uzyskuje dostęp do każdej właściwości ładowane z opóźnieniem jednostek, powodując jeszcze bardziej powolne ładowanie i serializacji. Aby uniknąć tego reakcję run-away, Włącz powolne ładowanie wyłączone przed serializacji jednostki.

Serializacja mogą również skomplikowane przez klasy serwera proxy, które korzysta z programu Entity Framework, zgodnie z objaśnieniem w [samouczek zaawansowane scenariusze](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Jednym ze sposobów, aby uniknąć problemów z serializacji jest do wykonywania serializacji obiektów transferu danych (dto) zamiast obiektów jednostek, jak pokazano w [przy użyciu interfejsu API sieci Web za pomocą platformy Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) samouczka.

Jeśli nie używasz dto, można wyłączyć ładowania z opóźnieniem i uniknąć problemów z serwera proxy, przez [wyłączenie serwera proxy tworzenia](https://msdn.microsoft.com/data/jj592886.aspx).

Poniżej przedstawiono niektóre inne [sposobów można wyłączyć ładowania z opóźnieniem](https://msdn.microsoft.com/data/jj574232):

- Dla właściwości nawigacji określonych, Pomiń `virtual` — słowo kluczowe w deklaracji właściwości.
- Wszystkie właściwości nawigacji, można ustawić `LazyLoadingEnabled` do `false`, umieść następujący kod w konstruktorze klasy kontekstu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Utwórz stronę kursy tej nazwy działu Wyświetla

`Course` Jednostki zawiera właściwość nawigacji, która zawiera `Department` jednostki działu, przypisana do kursu. Aby wyświetlić nazwę działu przypisany na liście kursy, musisz pobrać `Name` właściwość `Department` jednostki, która znajduje się w `Course.Department` właściwości nawigacji.

Utworzyć kontroler o nazwie `CourseController` (nie CoursesController) dla `Course` jednostki typu przy użyciu tych samych opcji dla **kontroler MVC 5 z widokami używający narzędzia Entity Framework** Generator szkieletu, która została wcześniej `Student` kontroler, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Otwórz *Controllers\CourseController.cs* i przyjrzyj się `Index` metody:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatyczne tworzenie szkieletów określono wczesne ładowanie dla `Department` właściwość nawigacji za pomocą `Include` metody.

Otwórz *Views\Course\Index.cshtml* i Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Utworzony szkielet kodu zostały wprowadzone następujące zmiany:

- Zmienione nagłówek z **indeksu** do **kursów**.
- Dodano **numer** kolumna, która pokazuje `CourseID` wartości właściwości. Domyślnie klucze podstawowe nie są szkieletu, ponieważ zwykle są bez znaczenia dla użytkowników końcowych. Jednak w takim przypadku klucz podstawowy ma znaczenie i chcą ją wyświetlić.
- Przenieść **działu** kolumny z prawej strony i zmienić jej nagłówek. Generator szkieletu poprawnie wybrał opcję wyświetlania `Name` właściwość `Department` jednostki, ale tutaj na stronie kurs nagłówek kolumny powinna być **działu** zamiast **nazwa**.

Należy zauważyć, że kolumny Dział utworzony szkielet kodu wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Uruchom stronę (wybierz **kursów** karty na stronie głównej University firmy Contoso) aby wyświetlić listę z nazwami działu.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Utwórz stronę instruktorów, pokazujący kursów i rejestracji

W tej sekcji utworzysz kontroler i wyświetlić `Instructor` jednostki w celu wyświetlenia strony Instruktorzy:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

- Lista Instruktorzy dane związane z `OfficeAssignment` jednostki. `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Użyjesz wczesne ładowanie dla `OfficeAssignment` jednostek. Jak wyjaśniono wcześniej, wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy będziesz potrzebować powiązanych danych we wszystkich wierszach pobrane w tabeli podstawowej. W tym przypadku chcesz wyświetlić przypisania pakietu office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze pod kierunkiem instruktora, związane z `Course` jednostki są wyświetlane. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Użyjesz wczesne ładowanie dla `Course` jednostek i ich powiązane `Department` jednostek. W tym przypadku powolne ładowanie może być bardziej efektywne, ponieważ należy kursy tylko dla wybranych przez instruktorów. Jednak w tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w ramach jednostek, które znajdują się w oknie właściwości nawigacji.
- Gdy użytkownik wybierze kurs, powiązane dane z `Enrollments` zostanie wyświetlony zestaw jednostek. `Course` i `Enrollment` jednostki są w relacji jeden do wielu. Należy dodać jawne ładowanie dla `Enrollment` jednostek i ich powiązane `Student` jednostek. (Jawne ładowanie jest to konieczne, ponieważ ładowania z opóźnieniem jest włączone, ale to pokazuje, jak to zrobić jawne ładowanie).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Tworzenie modelu widoku dla widoku indeksu przez instruktorów

Na stronie Instruktorzy wyświetlane trzech różnych tabelach. W związku z tym utworzysz zawiera trzy właściwości zawierający dane dla jednej z tabel, model widoku.

W *modele widoków* folderze utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Tworzenie widoków i kontrolerów przez instruktorów

Utwórz `InstructorController` (nie InstructorsController) kontroler z akcjami odczytu/zapisu EF, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Otwórz *Controllers\InstructorController.cs* i Dodaj `using` poufności informacji dotyczące `ViewModels` przestrzeni nazw:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Utworzony szkielet kodu w `Index` metody określa wczesne ładowanie tylko w przypadku `OfficeAssignment` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Zastąp `Index` metody za pomocą następującego kodu, aby załadować dodatkowe dane dotyczące i umieścić go w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Metoda akceptuje dane opcjonalne trasy (`id`) i parametr ciągu zapytania (`courseID`) podaj wartości identyfikator wybranego przez instruktorów i wybranych kursów i przekazuje wszystkie dane wymagane do widoku. Parametry są dostarczane przez **wybierz** hiperlinków na stronie.

Kod rozpoczyna się od tworzenia wystąpienia obiektu modelu widoku i umieszczenie w nim listy instruktorów. Kod ten określa wczesne ładowanie dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Drugi `Include` metoda ładuje kursów i każdego kursu, który jest ładowany robi wczesne ładowanie `Course.Department` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Jak wspomniano wcześniej, wczesne ładowanie nie jest wymagana, ale jest przeprowadzane w celu zwiększenia wydajności. Ponieważ widoku zawsze wymaga `OfficeAssignment` jednostki, jest bardziej wydajne, można pobrać, w tym samym zapytaniu. `Course` jednostki są wymagane w przypadku wybrania pod kierunkiem instruktora na stronie sieci web, więc wczesne ładowanie jest lepsze niż powolne ładowanie tylko wtedy, gdy ta strona jest wyświetlana częściej kurs wybrana niż bez.

Jeśli wybrano Identyfikatora przez instruktorów, wybranej przez instruktorów są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwość jest następnie ładowany z `Course` jednostek z tego instruktora `Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko jeden `Instructor` są zwracane jednostki. `Single` Metoda konwertuje kolekcję z jedną `Instructor` jednostki, która zapewnia dostęp do tej jednostki `Courses` właściwości.

Możesz użyć [pojedynczego](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metody w kolekcji, gdy wiadomo, Kolekcja będzie mieć tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja wymagał jest pusta lub ma więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w tym przypadku) Jeśli kolekcja jest pusta. Jednak w takim przypadku, będzie nadal prowadzić do wyjątku (z próby znalezienia `Courses` właściwość `null` odwołania), oraz komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Gdy wywołujesz `Single` metody, można również przekazać w `Where` warunku, zamiast wywoływać metodę `Where` metoda oddzielnie:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Następnie jeśli kurs został wybrany, wybranych kursów jest pobierana z Lista kursów model widoku. Następnie Wyświetl modelu `Enrollments` właściwość jest ładowany z `Enrollment` jednostek z tego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Zmodyfikuj widok indeksu przez instruktorów

W *Views\Instructor\Index.cshtml*, Zastąp kod szablonu poniższym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Następujące zmiany wprowadzone do istniejącego kodu:

- Klasa modelu, aby zmienić `InstructorIndexData`.
- Zmienić tytuł strony z **indeksu** do **Instruktorzy**.
- Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacja jeden do zero lub jeden, nie może być powiązane `OfficeAssignment` jednostki.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Dodano kod, który będzie dynamicznie dodawać `class="success"` do `tr` elementu wybranego przez instruktorów. To ustawienie koloru tła wybranego wiersza za pomocą [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) klasy. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Dodano nową `ActionLink` etykietą **wybierz** bezpośrednio przed innych linków w każdym wierszu, co powoduje, że identyfikator wybranego instruktora, które mają być wysyłane do `Index` metody.

Uruchom aplikację, a następnie wybierz **Instruktorzy** kartę. Zostanie wyświetlona strona `Location` powiązane właściwości `OfficeAssignment` jednostek i pustej tabeli komórki, gdy istnieje bez powiązanych `OfficeAssignment` jednostki.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

W *Views\Instructor\Index.cshtml* plików po upływie `table` — element (na końcu pliku), Dodaj następujący kod. Ten kod wyświetla listę kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także `Select` hiperłącze, które wysyła identyfikator wybranego kurs, aby `Index` metody akcji.

Uruchom strony i wybierz pod kierunkiem instruktora. Spowoduje to wyświetlenie siatce, która wyświetla kursy przypisane do wybranego przez instruktorów i każdego kursu możesz zobaczyć nazwę tego działu przypisane.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Spowoduje to wyświetlenie listy uczniów, którzy są rejestrowane kursu, po wybraniu danego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Ten kod odczytuje `Enrollments` właściwości modelu widoku, aby wyświetlić listę uczniów zarejestrowane w ramach tego kursu.

Uruchom strony i wybierz pod kierunkiem instruktora. Następnie wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Dodawanie jawne ładowanie

Otwórz *InstructorController.cs* i spójrz na sposób, w jaki `Index` metoda pobiera listę rejestracji dla wybranego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Po pobraniu listy instruktorów określony wczesne ładowanie dla `Courses` właściwość nawigacji i `Department` właściwości każdego kursu. A następnie przeniesiesz `Courses` kolekcji w modelu widoku, a teraz uzyskujesz dostęp do `Enrollments` właściwość nawigacji z jednej jednostki w tej kolekcji. Ponieważ nie określono wczesne ładowanie dla `Course.Enrollments` właściwość nawigacji, dane z tej właściwości jest wyświetlany na stronie wyniku powolne ładowanie.

Wyłączenie ładowania z opóźnieniem bez wpływu na kod w inny sposób `Enrollments` właściwość będzie o wartości null, niezależnie od tego, ile rejestracje kurs faktycznie miał. W takim przypadku można załadować `Enrollments` właściwości, trzeba określić wczesne ładowanie lub jawne ładowanie. Pokazaliśmy już już, jak zrobić wczesne ładowanie. Aby zobaczyć przykład jawne ładowanie, Zastąp `Index` metoda poniższym kodem, które jawnie ładuje `Enrollments` właściwości. Kod został zmieniony, zostały wyróżnione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Po otrzymaniu wybranego `Course` jednostki, nowy kod ładuje jawnie danego kursu `Enrollments` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Następnie są ładowane jawnie każdego `Enrollment` jednostki użytkownika związane z `Student` jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Zwróć uwagę, że używasz `Collection` metodę, aby załadować właściwość kolekcji, ale dla właściwości, która zawiera tylko jedną jednostkę, możesz użyć `Reference` metody.

Uruchom teraz strony indeksu przez instruktorów i ma różnicy w wyświetlanych na stronie zostanie wyświetlony, mimo że zostało zmienione, jak dane są pobierane.

## <a name="summary"></a>Podsumowanie

Znasz teraz wszystkie trzy sposoby (z opóźnieniem, eager i jawne) ładowanie powiązanych danych do właściwości nawigacji. W następnym samouczku dowiesz się, jak zaktualizować powiązane dane.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [dalej](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
