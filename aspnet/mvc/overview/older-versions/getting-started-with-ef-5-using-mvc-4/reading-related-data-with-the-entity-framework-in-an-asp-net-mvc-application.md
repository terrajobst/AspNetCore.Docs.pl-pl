---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Odczytywanie danych powiązanych z programu Entity Framework w aplikacji platformy ASP.NET MVC (5 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 831f5e0a8b57907ea012c10c1d1f8ff166f5e88b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Odczytywanie powiązane dane z programu Entity Framework w aplikacji platformy ASP.NET MVC (5, 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich instrukcji została ukończona modelu danych służbowych. W tym samouczku będziesz odczytu i wyświetlanie powiązanych danych, oznacza to, że dane programu Entity Framework ładuje do właściwości nawigacji.

Na poniższych ilustracjach przedstawiono stron, że będzie działać z.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Ładowanie opóźnieniem wczesny i jawne powiązanych danych

Istnieje kilka sposobów programu Entity Framework może ładować powiązanych danych do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Jednak podczas pierwszej próby dostępu do właściwości nawigacji wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Powoduje to wiele zapytań wysłanych do bazy danych — jeden dla sam podmiot i jeden musi zostać pobrany za każdym razem powiązane dane dla tej jednostki. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Ładowanie wczesny*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne. Określ wczesny ładowania przy użyciu `Include` metody.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowania*. Jest to podobne do opóźnionego ładowania, z wyjątkiem tego, że jawnie pobrać powiązanych danych w kodzie; go nie jest realizowane automatycznie podczas dostępu do właściwości nawigacji. Ręcznie załadować dane dotyczące pobierając wpis Menedżera stanu obiektu dla jednostki i wywoływania `Collection.Load` metody dla kolekcji lub `Reference.Load` metodę dla właściwości zawierających pojedynczej jednostki. (W poniższym przykładzie, aby załadować właściwość nawigacji administratora, należy zastąpić `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, opóźnionego ładowania i jawnego ładowania również zarówno nazywamy *odroczonego ładowania*.

Ogólnie rzecz biorąc Jeśli znasz potrzebne dane dotyczące dla każdy obiekt, do którego pobrane, wczesny ładowania zapewnia najlepszą wydajność, ponieważ pojedynczego zapytania wysyłane do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania dla każdej jednostki pobrane. Załóżmy na przykład, w powyższych przykładach, że każdy dział ma dziesięć Kursy pokrewne. W przykładzie wczesny ładowania spowoduje tylko zapytania jednym (połączenie) i jeden obiegu do bazy danych. Powolne ładowanie i przykłady jawnego ładowania zarówno spowoduje 11 zapytania i 11 rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwe dla wydajności, gdy dużych opóźnieniach.

Z drugiej strony w niektórych scenariuszach opóźnionego ładowania jest bardziej wydajny. Ładowanie wczesny może spowodować sprzężenia bardzo skomplikowane, zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli chcesz uzyskać dostęp do właściwości nawigacji jednostki tylko przez podzbiór zestawu jednostek w przypadku przetwarzania, opóźnionego ładowania mogą działać lepiej, ponieważ ładowanie wczesny czy pobrać więcej danych niż trzeba. Jeśli wydajność jest szczególnie ważne, najlepiej testowanie wydajności w obu kierunkach, aby ustawić najlepszym rozwiązaniem.

Zazwyczaj można użyć tylko wtedy, gdy została włączona ładowania poza opóźnionego jawnego ładowania. Jest jednym ze scenariuszy po ponownym włączeniu ładowania poza opóźnionego podczas serializacji. Opóźnionego ładowania i Serializacja nie wymieszaniu, a jeśli nie są dokładne, że może zakończyć się badania znacznie więcej danych niż zamierzony podczas opóźnieniem ładowania jest włączona. Serializacja zazwyczaj polega na uzyskiwanie dostępu do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala opóźnionego ładowania i podmioty załadować opóźnieniem są serializowane. Następnie procesu serializacji uzyskuje dostęp do każdej właściwości jednostek opóźnieniem załadować może powodować więcej opóźnionego ładowania i serializacji. Aby uniknąć tego łańcuchową run-away, Włącz opóźnionego ładowania wyłączone przed serializować jednostki.

Klasy kontekstu bazy danych wykonuje opóźnionego ładowania domyślnie. Istnieją dwa sposoby można wyłączyć ładowania opóźnionego:

- Dla właściwości nawigacji określonych, Pomiń `virtual` — słowo kluczowe w deklaracji właściwości.
- Dla wszystkich właściwości nawigacji, ustaw `LazyLoadingEnabled` do `false`. Na przykład można umieścić następujący kod w konstruktorze klasy kontekstu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Powolne ładowanie można zamaskować kodu, która powoduje występowanie problemów z wydajnością. Na przykład kodu, który nie określa wczesny lub jawnego ładowania, ale przetwarza dużą liczbę jednostek i używa kilku właściwości nawigacji w każdej iteracji może być bardzo mało wydajne (ze względu na wiele rund do bazy danych). Aplikacja, która wykonuje również Programowanie przy użyciu na lokalnym programem SQL server mogą wystąpić problemy z wydajnością po przeniesieniu do bazy danych SQL Azure z powodu większe opóźnienia i opóźnionego ładowania. Profilowanie zapytania bazy danych z realistyczne testu obciążenia pomoże określić, czy ładowanie opóźnieniem jest odpowiednia. Aby uzyskać więcej informacji, zobacz [Demystifying Entity Framework strategii: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [przy użyciu programu Entity Framework w celu zmniejszenia opóźnienia sieci SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Utwórz stronę indeksu kursy tego działu Wyświetla nazwę

`Course` Jednostki zawiera właściwość nawigacji, który zawiera `Department` jednostki działu przypisana do ciągu. Aby wyświetlić nazwę działu przypisane na liście kursów, należy uzyskać `Name` właściwość z `Department` jednostki, która znajduje się w `Course.Department` właściwości nawigacji.

Tworzenie kontrolera o nazwie `CourseController` dla `Course` typu jednostki, przy użyciu tych samych opcji, które zostało wcześniej dla `Student` kontrolera, jak pokazano na poniższej ilustracji (z wyjątkiem w odróżnieniu od obrazu, klasy kontekstu jest w przestrzeni nazw warstwy DAL nie modeli przestrzeni nazw):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Otwórz *Controllers\CourseController.cs* i przyjrzyj się `Index` metody:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatyczne szkieletów określono wczesny ładowania dla `Department` właściwość nawigacji za pomocą `Include` metody.

Otwórz *Views\Course\Index.cshtml* i Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Wprowadzono następujące zmiany do szkieletu kodu:

- Zmieniony nagłówek z **indeksu** do **kursów**.
- Przenieść łącza wiersza po lewej stronie.
- Dodaje kolumnę pod nagłówkiem **numer** którym wyświetlana jest `CourseID` wartość właściwości. (Domyślnie kluczy podstawowych nie są szkieletu ponieważ zwykle są bezużyteczne dla użytkowników końcowych. Jednak w takim przypadku klucza podstawowego ma znaczenie i chcesz je wyświetlić.)
- Zmienić ostatniego nagłówek kolumny z **DepartmentID** (nazwę klucza obcego do `Department` jednostek) do **działu**.

Zauważ, że dla ostatniej kolumny szkieletu kodu wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Uruchom strony (wybierz **kursów** kartę na stronie głównej University firmy Contoso) aby wyświetlić listę z nazwami działu.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Utwórz stronę indeksu instruktorów kursów i rejestracji

W tej sekcji zostanie utworzona kontrolera i wyświetlić dla `Instructor` jednostki w celu wyświetlenia strony indeksu instruktorów:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

- Lista instruktorów powiązane dane z `OfficeAssignment` jednostki. `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Użyjesz wczesny ładowania dla `OfficeAssignment` jednostek. Zgodnie z opisem, wczesny ładowania jest zazwyczaj bardziej wydajne, gdy potrzebne są powiązane dane dla wszystkich pobranych wierszy w tabeli podstawowej. W takim przypadku mają być wyświetlane przypisań pakietu office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze instruktora związane z `Course` wyświetlania obiektów. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Użyjesz wczesny ładowania dla `Course` jednostek i ich pokrewnych `Department` jednostek. W takim przypadku opóźnionego ładowania może być bardziej wydajne, ponieważ należy kursy tylko dla wybranego instruktora. Jednak ten przykład przedstawia sposób użycia wczesny ładowania dla właściwości nawigacji w ramach jednostkami, którymi na właściwości nawigacji.
- Po wybraniu kursu powiązane dane z `Enrollments` wyświetlany jest zestaw jednostek. `Course` i `Enrollment` jednostki są w relacji jeden do wielu. Należy dodać jawnego ładowania dla `Enrollment` jednostek i ich pokrewnych `Student` jednostek. (Podczas jawnego ładowania jest niezbędne, ponieważ opóźnionego ładowania jest włączona, ale oznacza to, jak to zrobić podczas jawnego ładowania).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Utwórz Model widoku dla widoku indeksu instruktora

Strona indeksu instruktora pokazuje trzech różnych tabel. W związku z tym utworzysz modelu widoku, który zawiera trzy właściwości, zawierający dane z tabel.

W *ViewModels* folderu, Utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Dodawanie stylu dla wierszy

Aby oznaczyć zaznaczone wiersze należy inny kolor tła. Zapewnienie stylu dla tego interfejsu użytkownika, Dodaj następujący wyróżniony kod do sekcji `/* info and errors */` w *Content\Site.css*, jak pokazano poniżej:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Tworzenie kontrolera instruktora i widoków

Utwórz `InstructorController` kontrolera, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Otwórz *Controllers\InstructorController.cs* i Dodaj `using` instrukcji dla `ViewModels` przestrzeni nazw:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Kod z utworzonym szkieletem w `Index` metody określa wczesny ładowania tylko w przypadku `OfficeAssignment` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Zastąp `Index` metody z następujący kod, aby załadować dodatkowe dane dotyczące i umieszcza je w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Metoda akceptuje dane trasy opcjonalne (`id`) i parametr ciągu zapytania (`courseID`) podaj wartości Identyfikatora wybranym instruktorze i wybranych przebieg i przekazuje wszystkie dane wymagane do widoku. Parametry są dostarczane przez **wybierz** hiperłącza, na stronie.

> [!TIP]
> 
> **Dane trasy**
> 
> Dane trasy to dane integratora modelu znaleziono w segment adresu URL określonego w tabeli routingu. Na przykład określa domyślną trasę `controller`, `action`, i `id` segmentów:
> 
> routes.MapRoute(  
>  Nazwa: "Default",  
>  adres URL: "{controller} / {action} / {id}",  
>  wartości domyślne: new {kontrolera = "Home", Akcja = "Index", id = UrlParameter.Optional}  
> );
> 
> Następujący adres URL, mapuje trasy domyślnej `Instructor` jako `controller`, `Index` jako `action` a 1 jako `id`; są wartości danych trasy.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" jest wartość ciągu zapytania. Integrator modelu również będzie działać w przypadku przekazania `id` jako wartość ciągu kwerendy:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Adresy URL są tworzone przez `ActionLink` instrukcje w widoku Razor. W poniższym kodzie `id` parametru odpowiada trasy domyślnej tak `id` jest dodawany do danych trasy.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> W poniższym kodzie `courseID` nie jest zgodny z parametrem w trasy domyślnej, jest dodawany jako ciąg zapytania.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Kod rozpoczyna się przez utworzenie wystąpienia modelu widoku i umieszczenie w nim listy instruktorów. Kod określa wczesny ładowania dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Drugi `Include` metody ładuje kursy i dla każdego kursu, który jest ładowany jest wczesny ładowania `Course.Department` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Jak wspomniano wcześniej, ładowanie wczesny nie jest wymagany, ale odbywa się w celu zwiększenia wydajności. Ponieważ widok zawsze wymaga `OfficeAssignment` jednostki, jest bardziej wydajne, można pobrać który w jednym zapytaniu. `Course` jednostki są wymagane w przypadku instruktora jest zaznaczona na stronie sieci web, ładowanie wczesny jest lepszym rozwiązaniem niż opóźnionego ładowania tylko wtedy, gdy strona jest wyświetlana częściej z kursu wybrane niż bez.

Jeśli wybrano Identyfikator instruktora, wybranym instruktorze są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwości jest następnie ładowany z `Course` jednostek z tego instruktora `Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko jeden `Instructor` są zwracane jednostki. `Single` Metoda konwertuje kolekcję do postaci jednej `Instructor` jednostki, która umożliwia dostęp do tej jednostki `Courses` właściwości.

Możesz użyć [pojedynczego](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metoda w kolekcji, gdy wiesz, Kolekcja będzie mieć tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja przekazywania jest pusta lub jeśli istnieje więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w takim przypadku), jeśli kolekcja jest pusta. Jednak w takim przypadku który nadal spowoduje powstanie wyjątku (z próby znalezienia `Courses` właściwość `null` odwołania), oraz komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Podczas wywoływania `Single` metody, można również przekazać `Where` warunku zamiast wywoływać metodę `Where` metody oddzielnie:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Obok Jeśli wybrano kursu, wybranego kursu są pobierane z listy kursy w modelu widoku. Następnie widok modelu `Enrollments` właściwości jest ładowany z `Enrollment` jednostek z tego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Zmodyfikowanie widoku indeksu instruktora

W *Views\Instructor\Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Następujące zmiany wprowadzone do istniejącego kodu:

- Klasa modelu, aby zmienić `InstructorIndexData`.
- Zmienić tytuł strony z **indeksu** do **instruktorów**.
- Przenieść kolumny łącza wiersza po lewej stronie.
- Usunięte **imię i nazwisko** kolumny.
- Dodaje **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie jest zerowa. (Ponieważ jest to relacji jeden do zero lub jeden, może nie być powiązanego `OfficeAssignment` jednostki.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Dodano kod, który będzie dynamicznie dodać `class="selectedrow"` do `tr` elementu wybranego instruktora. To ustawia kolor tła dla wybranego wiersza przy użyciu klasy CSS, który został utworzony wcześniej. ( `valign` Atrybutu będą przydatne w następujących instrukcji, podczas dodawania wielu kolumnę do tabeli.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Dodano nową `ActionLink` etykietą **wybierz** bezpośrednio przed innymi łączy w każdym wierszu, co powoduje, że identyfikator wybranego instruktora do wysłania do `Index` metody.

Uruchom aplikację i wybierz **instruktorów** kartę. Na stronie są wyświetlane `Location` powiązanych właściwości `OfficeAssignment` jednostek i pusta tabela komórki, gdy istnieje nr związane z `OfficeAssignment` jednostki.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

W *Views\Instructor\Index.cshtml* plików po upływie `table` elementu (na końcu pliku), Dodaj następujący wyróżniony kod. Spowoduje to wyświetlenie listy kursów związane z instruktora po wybraniu instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także `Select` hiperłącze, które wysyła identyfikator wybranego kursu do `Index` metody akcji.

> [!NOTE]
> *.Css* plik jest buforowany przez przeglądarki. Jeśli nie widzisz zmiany podczas uruchamiania aplikacji, czy twardych odświeżania (przytrzymaj klawisz CTRL podczas klikania **Odśwież** przycisk lub naciśnij klawisze CTRL + F5).


Uruchom strony i wybierz instruktora. Spowoduje to wyświetlenie Siatka wyświetla kursy przypisane do wybranych instruktora i dla każdego kursu wyświetlona nazwa przypisanej działu.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Lista zawiera studentów, którzy są zarejestrowane w toku w przypadku wybrania tego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Ten kod odczytuje `Enrollments` właściwości modelu widoku, aby wyświetlić listę studentów zarejestrowane w toku.

Uruchom strony i wybierz instruktora. Następnie wybierz plan, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Dodawanie jawnego ładowania

Otwórz *InstructorController.cs* i znajduje się w sposób `Index` metoda pobiera listę rejestracji dla porach wybranych:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Po pobraniu listy instruktorów określony wczesny ładowania dla `Courses` właściwość nawigacji i `Department` właściwości każdego kursu. Następnie możesz umieścić `Courses` kolekcji w modelu widoku, a teraz uzyskujesz dostęp do `Enrollments` właściwość nawigacji z jednego obiektu w tej kolekcji. Ponieważ nie określono wczesny ładowania dla `Course.Enrollments` właściwość nawigacji, w wyniku opóźnionego ładowania strony jest wyświetlane dane z tej właściwości.

Wyłączenie ładowania opóźnionego bez zmiany kodu w inny sposób `Enrollments` właściwość może mieć wartości null, niezależnie od tego, ile rejestracji kursu faktycznie ma. W takim przypadku można załadować `Enrollments` właściwość, należy określić wczesny ładowania lub jawnego ładowania. Już przedstawiono sposób wykonywania wczesny ładowania. Aby uzyskać przykład jawnego ładowania, Zastąp `Index` metodę z następującym kodem, które jawnie ładuje `Enrollments` właściwości. Wyróżniono kod został zmieniony.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Po pierwsze wybranego `Course` jednostki, nowy kod ładuje jawnie tego kursu `Enrollments` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Następnie ładuje jawnie każdego `Enrollment` jednostki użytkownika związane z `Student` jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Należy zauważyć, że używasz `Collection` metodę, aby załadować właściwość kolekcji, ale dla właściwości, który zawiera tylko jedną jednostkę, użyj `Reference` — metoda. Można teraz uruchomić strony indeksu instruktora i zobaczysz różnicy w wyświetlanych na stronie, mimo że zostały zmienione, jak dane są pobierane.

## <a name="summary"></a>Podsumowanie

Teraz używano wszystkie trzy sposoby (opóźnieniem wczesny i jawne) ładowanie powiązanych danych do właściwości nawigacji. W następnym samouczku nauczysz się, jak zaktualizować powiązanych danych.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [dalej](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
