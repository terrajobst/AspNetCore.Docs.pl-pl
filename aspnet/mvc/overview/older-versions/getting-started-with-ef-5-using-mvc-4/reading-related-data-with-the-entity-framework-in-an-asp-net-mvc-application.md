---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Odczytywanie danych powiązanych z platformą Entity Framework w aplikacji ASP.NET MVC (5, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 750c49572e99c6ab8c84d65e4c18f8da9b5d304c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835249"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Odczytywanie powiązanych danych, za pomocą programu Entity Framework w aplikacji ASP.NET MVC (5, 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku można wykonać modelu danych służbowych. W tym samouczku będziesz odczytywać i wyświetlanie powiązanych danych — oznacza to, że dane programu Entity Framework wczytywane właściwości nawigacji.

Na poniższych ilustracjach przedstawiono strony, którą będziesz pracować.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Z opóźnieniem, Eager i jawne ładowanie powiązanych danych

Istnieje kilka sposobów, platformy Entity Framework można załadować powiązane dane do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Podczas odczytywania jednostki powiązane dane nie są pobierane. Jednak użytkownik podejmie próbę dostępu do właściwości nawigacji po raz pierwszy wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Skutkiem wielu zapytań wysyłanych do bazy danych — jedną dla sam podmiot, a co za każdym razem, powiązane dane dla jednostki musi zostać pobrany. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Wczesne ładowanie*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne. Należy określić wczesne ładowanie za pomocą `Include` metody.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, chyba że jawnie pobrać powiązanych danych w kodzie nie będzie automatycznie gdy uzyskujesz dostęp do właściwości nawigacji. Ręczne ładowanie powiązanych danych uzyskując wpis Menedżera stanu obiektu do jednostki i wywoływania `Collection.Load` metody dla kolekcji lub `Reference.Load` metody, właściwości, które pełnią pojedynczej jednostki. (W poniższym przykładzie, jeśli chcesz załadować właściwość nawigacji administratora, należy zamienić `Collection(x => x.Courses)` z `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, powolne ładowanie i jawne ładowanie są również zarówno określane jako *odroczone ładowanie*.

Ogólnie rzecz biorąc Jeśli znasz należy powiązanych danych dla każdej jednostki, które pobrane, eager ładowanie zapewnia najlepszą wydajność, ponieważ pojedynczego zapytania wysłanego do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania, dla każdej jednostki pobierane. W powyższych przykładach, załóżmy, że każdy dział ma dziesięć Kursy pokrewne. W przykładzie wczesne ładowanie spowoduje tylko zapytania do jednego (Połącz) i jeden obiegu do bazy danych. Powolne ładowanie i przykłady jawne ładowanie zarówno spowodowałaby jedenaście zapytania i jedenaście rund do bazy danych. Dodatkowe rund do bazy danych są szczególnie szkodliwa dla wydajności, gdy opóźnienie jest wysokie.

Z drugiej strony w niektórych scenariuszach powolne ładowanie jest bardziej wydajne. Wczesne ładowanie może powodować bardzo złożone sprzężenia zostanie wygenerowany, której program SQL Server nie może przetworzyć wydajnie. Lub jeśli potrzebujesz uzyskać dostęp do właściwości nawigacji jednostki, tylko dla podzbioru zestawu jednostek w przypadku przetwarzania, powolne ładowanie może działać lepiej, ponieważ wczesne ładowanie może pobrać większej ilości danych niż jest Ci potrzebne. Jeśli wydajność ma kluczowe znaczenie, najlepiej testowania wydajności w obu kierunkach, aby można było dokonanie najlepszego wyboru.

Zazwyczaj należy użyć jawne ładowanie tylko wtedy, gdy ta wiadomość została wysłana z opóźnieniem, ładowanie wyłączone. To jeden scenariusz po ponownym włączeniu powolne ładowanie wyłączone podczas serializacji. Powolne ładowanie i serializacji nie Mieszaj dobrze, a jeśli nie chcesz zachować ostrożność, że użytkownik może pozostać zapytań znacznie większej ilości danych niż planowano, gdy z opóźnieniem ładowania jest włączona. Serializacja zazwyczaj polega na dostęp do każdej właściwości w wystąpieniu typu. Dostęp do właściwości wyzwala powolne ładowanie i podmioty załadowanych z opóźnieniem są serializowane. Następnie procesem serializacji uzyskuje dostęp do każdej właściwości ładowane z opóźnieniem jednostek, powodując jeszcze bardziej powolne ładowanie i serializacji. Aby uniknąć tego reakcję run-away, Włącz powolne ładowanie wyłączone przed serializacji jednostki.

Klasy kontekstu bazy danych wykonuje powolne ładowanie domyślnie. Istnieją dwa sposoby, aby wyłączyć ładowanie z opóźnieniem:

- Dla właściwości nawigacji określonych, Pomiń `virtual` — słowo kluczowe w deklaracji właściwości.
- Wszystkie właściwości nawigacji, można ustawić `LazyLoadingEnabled` do `false`. Na przykład poniższy kod można umieścić w konstruktorze klasy kontekstu: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Powolne ładowanie może maskować kod, który powoduje, że problemy z wydajnością. Na przykład kod, który nie określa eager lub jawny ładowania, ale przetwarza dużą liczbę jednostek i wykorzystuje kilka właściwości nawigacji w każdej iteracji może być nieefektywna (ze względu na wiele rund do bazy danych). Aplikacja, która wykonuje również Programowanie przy użyciu serwera SQL na lokalnych mogą wystąpić problemy z wydajnością po przeniesieniu do usługi Azure SQL Database ze względu na większe opóźnienia i ładowania z opóźnieniem. Profilowanie zapytania do bazy danych przy użyciu realistycznej testu obciążenia pomoże określić, czy powolne ładowanie jest odpowiednia. Aby uzyskać więcej informacji, zobacz [Demystifying strategie programu Entity Framework: ładowanie powiązanych danych](https://msdn.microsoft.com/magazine/hh205756.aspx) i [używający narzędzia Entity Framework w celu zmniejszenia opóźnienia sieci na platformie Azure SQL](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Utwórz stronę indeksu kursy tej nazwy działu Wyświetla

`Course` Jednostki zawiera właściwość nawigacji, która zawiera `Department` jednostki działu, przypisana do kursu. Aby wyświetlić nazwę działu przypisany na liście kursy, musisz pobrać `Name` właściwość `Department` jednostki, która znajduje się w `Course.Department` właściwości nawigacji.

Utworzyć kontroler o nazwie `CourseController` dla `Course` jednostki typu przy użyciu tych samych opcji, które miało to miejsce wcześniej dla `Student` kontroler, jak pokazano na poniższej ilustracji (z wyjątkiem w przeciwieństwie do obrazu, klasy kontekstu jest w przestrzeni nazw warstwy DAL nie modeli przestrzeni nazw):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Otwórz *Controllers\CourseController.cs* i przyjrzyj się `Index` metody:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Automatyczne tworzenie szkieletów określono wczesne ładowanie dla `Department` właściwość nawigacji za pomocą `Include` metody.

Otwórz *Views\Course\Index.cshtml* i Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Utworzony szkielet kodu zostały wprowadzone następujące zmiany:

- Zmienione nagłówek z **indeksu** do **kursów**.
- Przenieść łącza wiersza po lewej stronie.
- Dodaje kolumnę o nagłówku **numer** pokazujący `CourseID` wartości właściwości. (Domyślnie klucze podstawowe nie są szkieletu ponieważ zwykle są bez znaczenia dla użytkowników końcowych. Jednak w takim przypadku klucz podstawowy ma znaczenie i chcą ją wyświetlić.)
- Zmienić ostatnie nagłówek kolumny z **DepartmentID** (nazwa klucz obcy, aby `Department` jednostek) do **działu**.

Należy zauważyć, że ostatniej kolumny utworzony szkielet kodu wyświetla `Name` właściwość `Department` jednostki, który jest ładowany do `Department` właściwość nawigacji:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Uruchom stronę (wybierz **kursów** karty na stronie głównej University firmy Contoso) aby wyświetlić listę z nazwami działu.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Utwórz stronę indeksu Instruktorzy pokazujący kursów i rejestracji

W tej sekcji utworzysz kontroler i wyświetlić `Instructor` jednostki w celu wyświetlenia strony indeksu Instruktorzy:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ta strona odczytuje i wyświetla powiązanych danych w następujący sposób:

- Lista Instruktorzy dane związane z `OfficeAssignment` jednostki. `Instructor` i `OfficeAssignment` jednostki są w relacji jeden do zero lub jeden. Użyjesz wczesne ładowanie dla `OfficeAssignment` jednostek. Jak wyjaśniono wcześniej, wczesne ładowanie jest zazwyczaj bardziej efektywne, gdy będziesz potrzebować powiązanych danych we wszystkich wierszach pobrane w tabeli podstawowej. W tym przypadku chcesz wyświetlić przypisania pakietu office dla wszystkich wyświetlanych instruktorów.
- Gdy użytkownik wybierze pod kierunkiem instruktora, związane z `Course` jednostki są wyświetlane. `Instructor` i `Course` jednostki są w relacji wiele do wielu. Użyjesz wczesne ładowanie dla `Course` jednostek i ich powiązane `Department` jednostek. W tym przypadku powolne ładowanie może być bardziej efektywne, ponieważ należy kursy tylko dla wybranych przez instruktorów. Jednak w tym przykładzie pokazano, jak używać wczesne ładowanie dla właściwości nawigacji w ramach jednostek, które znajdują się w oknie właściwości nawigacji.
- Gdy użytkownik wybierze kurs, powiązane dane z `Enrollments` zostanie wyświetlony zestaw jednostek. `Course` i `Enrollment` jednostki są w relacji jeden do wielu. Należy dodać jawne ładowanie dla `Enrollment` jednostek i ich powiązane `Student` jednostek. (Jawne ładowanie jest to konieczne, ponieważ ładowania z opóźnieniem jest włączone, ale to pokazuje, jak to zrobić jawne ładowanie).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Tworzenie modelu widoku dla widoku indeksu przez instruktorów

Strona indeksu przez instruktorów zawiera trzech różnych tabelach. W związku z tym utworzysz zawiera trzy właściwości zawierający dane dla jednej z tabel, model widoku.

W *modele widoków* folderze utwórz *InstructorIndexData.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Dodawanie stylu dla wybranych wierszy

Aby oznaczyć wybrane wiersze, musisz mieć inny kolor tła. Aby zapewnić stylu tego interfejsu użytkownika, Dodaj następujący wyróżniony kod do sekcji `/* info and errors */` w *Content\Site.css*, jak pokazano poniżej:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Tworzenie kontrolera przez instruktorów i widoków

Utwórz `InstructorController` kontroler, jak pokazano na poniższej ilustracji:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Otwórz *Controllers\InstructorController.cs* i Dodaj `using` poufności informacji dotyczące `ViewModels` przestrzeni nazw:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Utworzony szkielet kodu w `Index` metody określa wczesne ładowanie tylko w przypadku `OfficeAssignment` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Zastąp `Index` metody za pomocą następującego kodu, aby załadować dodatkowe dane dotyczące i umieścić go w modelu widoku:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Metoda akceptuje dane opcjonalne trasy (`id`) i parametr ciągu zapytania (`courseID`) podaj wartości identyfikator wybranego przez instruktorów i wybranych kursów i przekazuje wszystkie dane wymagane do widoku. Parametry są dostarczane przez **wybierz** hiperlinków na stronie.

> [!TIP]
> 
> **Dane trasy**
> 
> Przekierowywanie danych to dane, które w segment adresu URL określony w tabeli routingu znaleziono integratora modelu. Na przykład określa domyślną trasę `controller`, `action`, i `id` segmenty:
> 
> routes.MapRoute(  
>  Nazwa: "Default"  
>  adres URL: "{controller} / {action} / {id}",  
>  wartości domyślne: nowe {kontrolera = "Home", Akcja = "Index", identyfikator = UrlParameter.Optional}  
> );
> 
> Następujący adres URL mapuje trasy domyślnej `Instructor` jako `controller`, `Index` jako `action` i 1 `id`; są to wartości danych trasy.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" jest wartością ciągu zapytania. Integrator modelu również sprawdzi się w przypadku przekazania `id` jako wartość ciągu zapytania:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Adresy URL są tworzone przez `ActionLink` instrukcji w widoku Razor. W poniższym kodzie `id` parametr odpowiada trasa domyślna, więc `id` jest dodawany do kierowania danych.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> W poniższym kodzie `courseID` nie jest zgodny z parametrem dla trasy domyślne, więc zostanie dodany jako ciąg zapytania.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Kod rozpoczyna się od tworzenia wystąpienia obiektu modelu widoku i umieszczenie w nim listy instruktorów. Kod ten określa wczesne ładowanie dla `Instructor.OfficeAssignment` i `Instructor.Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Drugi `Include` metoda ładuje kursów i każdego kursu, który jest ładowany robi wczesne ładowanie `Course.Department` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Jak wspomniano wcześniej, wczesne ładowanie nie jest wymagana, ale jest przeprowadzane w celu zwiększenia wydajności. Ponieważ widoku zawsze wymaga `OfficeAssignment` jednostki, jest bardziej wydajne, można pobrać, w tym samym zapytaniu. `Course` jednostki są wymagane w przypadku wybrania pod kierunkiem instruktora na stronie sieci web, więc wczesne ładowanie jest lepsze niż powolne ładowanie tylko wtedy, gdy ta strona jest wyświetlana częściej kurs wybrana niż bez.

Jeśli wybrano Identyfikatora przez instruktorów, wybranej przez instruktorów są pobierane z listy w modelu widoku instruktorów. Model widoku `Courses` właściwość jest następnie ładowany z `Course` jednostek z tego instruktora `Courses` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

`Where` Metoda zwraca kolekcję, ale w takim przypadku kryteria przekazany do tej metody powodują tylko jeden `Instructor` są zwracane jednostki. `Single` Metoda konwertuje kolekcję z jedną `Instructor` jednostki, która zapewnia dostęp do tej jednostki `Courses` właściwości.

Możesz użyć [pojedynczego](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metody w kolekcji, gdy wiadomo, Kolekcja będzie mieć tylko jeden element. `Single` Metoda zgłasza wyjątek, jeśli kolekcja wymagał jest pusta lub ma więcej niż jeden element. Alternatywą jest [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), która zwraca wartość domyślną (`null` w tym przypadku) Jeśli kolekcja jest pusta. Jednak w takim przypadku, będzie nadal prowadzić do wyjątku (z próby znalezienia `Courses` właściwość `null` odwołania), oraz komunikat o wyjątku mniej wyraźnie wskazuje przyczynę problemu. Gdy wywołujesz `Single` metody, można również przekazać w `Where` warunku, zamiast wywoływać metodę `Where` metoda oddzielnie:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Zamiast:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Następnie jeśli kurs został wybrany, wybranych kursów jest pobierana z Lista kursów model widoku. Następnie Wyświetl modelu `Enrollments` właściwość jest ładowany z `Enrollment` jednostek z tego kursu `Enrollments` właściwości nawigacji.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modyfikowanie widoku indeksu przez instruktorów

W *Views\Instructor\Index.cshtml*, Zastąp istniejący kod następującym kodem. Zmiany są wyróżnione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Następujące zmiany wprowadzone do istniejącego kodu:

- Klasa modelu, aby zmienić `InstructorIndexData`.
- Zmienić tytuł strony z **indeksu** do **Instruktorzy**.
- Przenieść kolumny wiersza link po lewej stronie.
- Usunięte **imię i nazwisko** kolumny.
- Dodano **Office** kolumnę wyświetlającą `item.OfficeAssignment.Location` tylko wtedy, gdy `item.OfficeAssignment` nie ma wartości null. (Ponieważ jest to relacja jeden do zero lub jeden, nie może być powiązane `OfficeAssignment` jednostki.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Dodano kod, który będzie dynamicznie dodawać `class="selectedrow"` do `tr` elementu wybranego przez instruktorów. To ustawienie jako kolor tła wybranego wiersza przy użyciu klasy CSS, która została utworzona wcześniej. ( `valign` Atrybut będą przydatne do następującego samouczka, podczas dodawania kolumny z wieloma wierszami w tabeli.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Dodano nową `ActionLink` etykietą **wybierz** bezpośrednio przed innych linków w każdym wierszu, co powoduje, że identyfikator wybranego instruktora, które mają być wysyłane do `Index` metody.

Uruchom aplikację, a następnie wybierz **Instruktorzy** kartę. Zostanie wyświetlona strona `Location` powiązane właściwości `OfficeAssignment` jednostek i pustej tabeli komórki, gdy istnieje bez powiązanych `OfficeAssignment` jednostki.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

W *Views\Instructor\Index.cshtml* plików po upływie `table` — element (na końcu pliku), Dodaj następujący wyróżniony kod. Spowoduje to wyświetlenie listy kursów związane z kierunkiem instruktora, po wybraniu pod kierunkiem instruktora.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Ten kod odczytuje `Courses` właściwości modelu widoku, aby wyświetlić listę kursów. Zapewnia także `Select` hiperłącze, które wysyła identyfikator wybranego kurs, aby `Index` metody akcji.

> [!NOTE]
> *.Css* plików jest buforowana przez przeglądarki. Jeśli zmiany nie jest widoczny po uruchomieniu aplikacji, wykonaj twardych odświeżania (przytrzymaj wciśnięty klawisz CTRL podczas klikania **Odśwież** przycisk lub naciśnij klawisze CTRL + F5).


Uruchom strony i wybierz pod kierunkiem instruktora. Spowoduje to wyświetlenie siatce, która wyświetla kursy przypisane do wybranego przez instruktorów i każdego kursu możesz zobaczyć nazwę tego działu przypisane.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Po bloku kodu, który właśnie został dodany Dodaj następujący kod. Spowoduje to wyświetlenie listy uczniów, którzy są rejestrowane kursu, po wybraniu danego kursu.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Ten kod odczytuje `Enrollments` właściwości modelu widoku, aby wyświetlić listę uczniów zarejestrowane w ramach tego kursu.

Uruchom strony i wybierz pod kierunkiem instruktora. Następnie wybierz kurs, aby wyświetlić listę zarejestrowanych studentów i ich klas.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Dodawanie jawne ładowanie

Otwórz *InstructorController.cs* i spójrz na sposób, w jaki `Index` metoda pobiera listę rejestracji dla wybranego kursu:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Po pobraniu listy instruktorów określony wczesne ładowanie dla `Courses` właściwość nawigacji i `Department` właściwości każdego kursu. A następnie przeniesiesz `Courses` kolekcji w modelu widoku, a teraz uzyskujesz dostęp do `Enrollments` właściwość nawigacji z jednej jednostki w tej kolekcji. Ponieważ nie określono wczesne ładowanie dla `Course.Enrollments` właściwość nawigacji, dane z tej właściwości jest wyświetlany na stronie wyniku powolne ładowanie.

Wyłączenie ładowania z opóźnieniem bez wpływu na kod w inny sposób `Enrollments` właściwość będzie o wartości null, niezależnie od tego, ile rejestracje kurs faktycznie miał. W takim przypadku można załadować `Enrollments` właściwości, trzeba określić wczesne ładowanie lub jawne ładowanie. Pokazaliśmy już już, jak zrobić wczesne ładowanie. Aby zobaczyć przykład jawne ładowanie, Zastąp `Index` metoda poniższym kodem, które jawnie ładuje `Enrollments` właściwości. Kod został zmieniony, zostały wyróżnione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Po otrzymaniu wybranego `Course` jednostki, nowy kod ładuje jawnie danego kursu `Enrollments` właściwość nawigacji:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Następnie są ładowane jawnie każdego `Enrollment` jednostki użytkownika związane z `Student` jednostki:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Zwróć uwagę, że używasz `Collection` metodę, aby załadować właściwość kolekcji, ale dla właściwości, która zawiera tylko jedną jednostkę, możesz użyć `Reference` metody. Możesz teraz uruchomić stronę indeksu przez instruktorów i ma różnicy w wyświetlanych na stronie zostanie wyświetlony, mimo że zostało zmienione, jak dane są pobierane.

## <a name="summary"></a>Podsumowanie

Znasz teraz wszystkie trzy sposoby (z opóźnieniem, eager i jawne) ładowanie powiązanych danych do właściwości nawigacji. W następnym samouczku dowiesz się, jak zaktualizować powiązane dane.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [dalej](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
