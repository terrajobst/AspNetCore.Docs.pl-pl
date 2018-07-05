---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementowanie podstawowych funkcji CRUD z platformą Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eeed2b572ddecb66c3b95926b57dd783c26d5f0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390220"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementowanie podstawowych funkcji CRUD z platformą Entity Framework w aplikacji ASP.NET MVC
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [Pobierz plik PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 przy użyciu Entity Framework 6 Code First i Visual Studio 2013. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednim samouczku utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu platformy Entity Framework i programu SQL Server LocalDB. W tym samouczku będziesz przejrzenie i dostosowanie CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) kod, który MVC scaffolding automatycznie utworzy dla Ciebie w widoków i kontrolerów.

> [!NOTE]
> Jest to powszechną praktyką w celu zaimplementowania wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem i warstwy dostępu do danych. Aby zachować te samouczki, prosty i skupiają się na nauczania, jak używać programu Entity Framework sam, nie używają repozytoriów. Aby uzyskać informacje o sposobie wdrażania repozytoriów, zobacz [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).


W tym samouczku utworzysz następujących stron sieci web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Utwórz stronę szczegółów

Utworzony szkielet kodu dla uczniów lub studentów `Index` strony pozostawiono `Enrollments` właściwość, ponieważ ta właściwość zawiera kolekcję. W `Details` strony zawartości kolekcji będą wyświetlane w tabeli HTML.

 W *Controllers\StudentController.cs*, metodę akcji dla `Details` wyświetlić używa [znaleźć](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) metodę, aby pobrać jeden `Student` jednostki. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Wartość klucza jest przekazywany do metody jako `id` parametru i pochodzi z *kierować dane* w **szczegóły** hiperłącze na stronę indeksu.

### <a name="tip-route-data"></a>Porada: **przekierowywanie danych**

Przekierowywanie danych to dane, które w segment adresu URL określony w tabeli routingu znaleziono integratora modelu. Na przykład określa domyślną trasę `controller`, `action`, i `id` segmenty:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Następujący adres URL mapuje trasy domyślnej `Instructor` jako `controller`, `Index` jako `action` i 1 `id`; są to wartości danych trasy.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" jest wartością ciągu zapytania. Integrator modelu również sprawdzi się w przypadku przekazania `id` jako wartość ciągu zapytania:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Adresy URL są tworzone przez `ActionLink` instrukcji w widoku Razor. W poniższym kodzie `id` parametr odpowiada trasa domyślna, więc `id` jest dodawany do kierowania danych.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

W poniższym kodzie `courseID` nie jest zgodny z parametrem dla trasy domyślne, więc zostanie dodany jako ciąg zapytania.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Otwórz *Views\Student\Details.cshtml*. Każde pole jest wyświetlana przy użyciu `DisplayFor` pomocnika, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Po `EnrollmentDate` pól i bezpośrednio przed tagiem zamykającym `</dl>` tagów, należy dodać wyróżniony kod, aby wyświetlić listę rejestracji, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    W przypadku wcięcia kodu o szerokość problem po wklejeniu kodu, naciśnij kombinację klawiszy CTRL-K-D go poprawić.

    Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego `Enrollment` jednostki we właściwości Wyświetla tytuł kurs i klasy korporacyjnej. Tytuł kurs jest pobierana z `Course` jednostki, która jest przechowywana w `Course` właściwość nawigacji `Enrollments` jednostki. Wszystkie te dane są pobierane z bazy danych automatycznie, gdy jest to konieczne. (Innymi słowy, używane są powolne ładowanie tutaj. Nie określono *wczesne ładowanie* dla `Courses` właściwość nawigacji, więc rejestracji nie zostały pobrane w jednym zapytaniu, która otrzymała uczniów. Zamiast tego po raz pierwszy próbujesz uzyskać dostęp do `Enrollments` właściwość nawigacji nowe zapytanie jest wysyłane do pobierania danych do bazy danych. Możesz dowiedzieć się więcej o powolne ładowanie i wczesne ładowanie w [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.)
3. Uruchomienia strony, wybierając **studentów** kartę i klikając **szczegóły** linku, aby użytkownik Alexander Carson. (Jeśli naciśniesz kombinację klawiszy CTRL + F5, otwarciu pliku Details.cshtml, otrzymasz błąd HTTP 400 ponieważ Visual Studio podejmie próbę uruchomienia na stronie szczegółów, ale go nie można nawiązać kontaktu z link, który określa ucznia, aby wyświetlić. In that Case, po prostu usuń "Dla uczniów/Details" z adresu URL i spróbuj ponownie, lub zamknij przeglądarkę, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **widoku**, a następnie kliknij przycisk **Pokaż w przeglądarce**.)

    Zostanie wyświetlona lista kursów i ocen dla wybranych uczniów lub studentów:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

1. W *Controllers\StudentController.cs*, Zastąp `HttpPost``Create` metody akcji, używając następującego kodu, aby dodać `try-catch` blokowania i Usuń `ID` z [atrybut wiązania](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) dla Metoda szkieletu:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ten kod dodaje `Student` jednostki utworzone przez integrator modelu programu ASP.NET MVC do `Students` jednostki zestawu, a następnie zapisuje zmiany w bazie danych. (*Integratora modelu* odwołuje się do funkcjonalność platformy ASP.NET MVC, która pozwala je łatwiej jest pracować z danych przesyłanych przez formularz; integratora modelu w postaci konwertuje opublikowane wartości na typy CLR i przekazuje je do metody akcji w parametrach. In this case, tworzy wystąpienie integratora modelu `Student` jednostki przy użyciu właściwości wartości z kolekcji `Form` kolekcji.)

    Możesz usunąć `ID` z powiązania atrybutu, ponieważ `ID` jest wartość klucza podstawowego, który program SQL Server ustawi automatycznie, gdy zostanie wstawiona. Dane wejściowe od użytkownika nie ustawia `ID` wartość.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Ostrzeżenie o zabezpieczeniach - `ValidateAntiForgeryToken` atrybutów pomaga zapobiec [fałszerstwo żądania międzywitrynowego](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków. Wymaga ona odpowiednią `Html.AntiForgeryToken()` instrukcji w widoku, który pojawi się później.

    `Bind` Atrybut jest jednym ze sposobów, aby zapewnić ochronę przed *polegającymi* w tworzenie scenariuszy. Na przykład, załóżmy, że `Student` zawiera jednostki `Secret` właściwości, które nie mają tej strony sieci web, aby ustawić.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Nawet jeśli nie masz `Secret` pola na stronie sieci web haker może użyć narzędzia takie jak [fiddler](http://fiddler2.com/home), lub napisz fragmentów kodu JavaScript do opublikowania `Secret` stanowią wartość. Bez [powiązać](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atrybut ograniczanie pól używanych przez integrator modelu podczas tworzenia `Student` wystąpienia<em>,</em> integratora modelu czy wczytać, `Secret` formę wartość i użyć go do Utwórz `Student` wystąpienia jednostki. Następnie niezależnie od wartości haker, określony dla `Secret` pola formularza zostałaby zaktualizowana w bazie danych. Na poniższej ilustracji przedstawiono program fiddler Dodawanie narzędzie `Secret` pola (z wartością "OverPost") do wartości przesłanego formularza.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Wartość "OverPost" następnie zostaną pomyślnie dodane do `Secret` właściwość wstawionego wiersza, ale nigdy nie ma strony sieci web i można ustawić tę właściwość.

    Jest najlepszym rozwiązaniem w zakresie zabezpieczeń użyj `Include` parametrem `Bind` atrybutu *dozwolonych* pola. Istnieje również możliwość użycia `Exclude` parametr *blacklist* pola, które chcesz wykluczyć. Przyczyna `Include` zapewnia większe bezpieczeństwo jest, że po dodaniu nowej właściwości do jednostki, nowe pole nie jest automatycznie chroniona przez `Exclude` listy.

    Można zapobiec overposting w scenariuszach edycji jest najpierw odczytu jednostki z bazy danych, a następnie wywołując `TryUpdateModel`, przekazując jako jawne dozwolonych właściwości listy tych praktyk. To metodę używaną w tych samouczkach.

    Alternatywny sposób, aby zapobiec overposting, który jest wybierany przez wielu deweloperów jest wyświetlanie modeli, a nie klas jednostek za pomocą wiązania modelu. Zawierają właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu integratora modelu MVC skopiuj Wyświetl właściwości modelu do wystąpienia jednostki, takie jak opcjonalnie za pomocą narzędzia [AutoMapper](http://automapper.org/). Użyj bazy danych. Wpis w jednostce wystąpienia, aby ustawić jej stan Unchanged, a następnie ustaw Property("PropertyName"). IsModified na wartość true dla każdej właściwości jednostki, który znajduje się w modelu widoku. Ta metoda działa, w obu edytowanie i tworzenie scenariuszy.

    Inne niż `Bind` atrybutu `try-catch` blok jest tylko zmiany wprowadzone do utworzony szkielet kodu. Jeśli wyjątek, który pochodzi od klasy [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) jest przechwycony podczas zmiany są zapisywane, jest wyświetlany ogólny komunikat o błędzie. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) wyjątki są czasami skutkiem coś zewnętrznego do aplikacji, a nie błąd programowania, dzięki czemu użytkownik jest zalecane, aby spróbować ponownie. Chociaż nie jest zaimplementowana w tym przykładzie, aplikacji jakości produkcyjnej rejestruje wyjątek. Aby uzyskać więcej informacji, zobacz **dziennik, aby uzyskać szczegółowe informacje o** sekcji [monitorowanie i Telemetria (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kod w *Views\Student\Create.cshtml* jest podobny do przedstawionego w *Details.cshtml*, chyba że `EditorFor` i `ValidationMessageFor` pomocników są używane dla każdego pola, zamiast `DisplayFor`. W tym miejscu jest odpowiedni kod:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* obejmuje również `@Html.AntiForgeryToken()`, który współdziała z `ValidateAntiForgeryToken` atrybutu w kontrolerze w celu zapobieżenia [fałszerstwo żądania międzywitrynowego](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków.

    Żadne zmiany nie są wymagane w *Create.cshtml*.
2. Uruchom stronę, wybierając **studentów** kartę i klikając **Utwórz nowy**.
3. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** aby zobaczyć komunikat o błędzie.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Jest to sprawdzanie poprawności po stronie serwera, które można pobrać domyślnej; później w samouczku pokazano, jak dodać atrybuty, które również spowoduje to wygenerowanie kodu do weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu w **Utwórz** metody.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Zmień datę na prawidłową wartość, a następnie kliknij przycisk **Utwórz** do nowego studenta pojawiają się w temacie **indeksu** strony.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Metoda HttpPost edycji aktualizacji

W *Controllers\StudentController.cs*, `HttpGet` `Edit` — metoda (jeden bez `HttpPost` atrybutu) używa `Find` metody do pobierania wybranej `Student` jednostki, jak pokazano w `Details` metody. Nie trzeba zmienić tę metodę.

Jednak zastąpić `HttpPost` `Edit` metody akcji, używając następującego kodu:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Te zmiany zaimplementować najlepszym rozwiązaniem w zakresie zabezpieczeń zapobiec [polegającymi](#overpost), generator szkieletu generowane `Bind` atrybutu i dodać jednostki utworzone przez integratora modelu do zestawu z flagą zmodyfikowane jednostek. Czy kod nie jest zalecane, ponieważ `Bind` atrybutu powoduje danych zapisanych w polach, które nie są wymienione w `Include` parametru. W przyszłości, generator szkieletu kontrolera MVC zostanie zaktualizowana tak, aby nie generować `Bind` atrybuty dla metod edycji.

Nowy kod odczytuje istniejącej jednostki i wywołania [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) można zaktualizować pola z danych wejściowych użytkownika w danych przesłanego formularza. Entity Framework automatyczne śledzenie zestawy zmian [zmodyfikowane](https://msdn.microsoft.com/library/system.data.entitystate.aspx) flagi w jednostce. Gdy [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda jest wywoływana, `Modified` flaga powoduje, że platforma Entity Framework do tworzenia instrukcji SQL, aby zaktualizować wiersz bazy danych. [Konflikty współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) są ignorowane, a wszystkie kolumny wiersza bazy danych zostaną zaktualizowane, łącznie z tymi, które użytkownik nie został zmieniony. (Dalszych samouczków pokazano, jak obsługa konfliktów współbieżności, a jeśli chcesz tylko poszczególne pola do zaktualizowania w bazie danych, możesz ustawić jednostki do Unchanged i ustawić poszczególnych pól w celu zmodyfikowane.)

Najlepszym rozwiązaniem, aby zapobiec overposting pola, które mają być można aktualizować przez stronę edycji są na liście dozwolonych w `TryUpdateModel` parametrów. Aktualnie nie istnieją żadne dodatkowe pola, które chronisz, ale lista pól, które chcesz integratora modelu do powiązania gwarantuje, że po dodaniu pola do modelu danych w przyszłości, są automatycznie chroniona, dopóki nie dodasz je tutaj.

W wyniku tych zmian podpis metody metody HttpPost edycji jest taka sama jak metoda edycji HttpGet; w związku z tym po zmianie nazwy metody EditPost.

> [!TIP] 
> 
> **Stany jednostki i dołączania i metod SaveChanges**
> 
> Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych, a te informacje określa, co się stanie, gdy wywołujesz `SaveChanges` metody. Na przykład podczas przekazywania do nowej jednostki [Dodaj](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metody, która stan obiektu jest ustawiony na `Added`. Następnie, gdy zostanie wywołana [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody wystawia kontekst bazy danych SQL `INSERT` polecenia.
> 
> Jednostki mogą być w jednym z[następujące stany](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`. Jednostka nie istnieje jeszcze w bazie danych. `SaveChanges` Metody należy wygenerować `INSERT` instrukcji.
> - `Unchanged`. Niczego nie trzeba wykonać za pomocą tej jednostki przy `SaveChanges` metody. Podczas odczytywania jednostki z bazy danych, jednostka rozpoczyna się w tym stanie.
> - `Modified`. Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. `SaveChanges` Metody należy wygenerować `UPDATE` instrukcji.
> - `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metody należy wygenerować `DELETE` instrukcji.
> - `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.
> 
> W przypadku aplikacji komputerowych zmiany stanu są zazwyczaj ustawiane automatycznie. Pulpitu typu aplikacji służy do odczytu jednostki i wprowadzić zmiany do niektórych wartości właściwości. Powoduje to, że jego stan jednostki automatycznie był zmieniany na `Modified`. Następnie, gdy zostanie wywołana `SaveChanges`, platformy Entity Framework generuje SQL `UPDATE` instrukcję, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.
> 
> Odłączony rodzaj aplikacji sieci web nie pozwala na to ciągła sekwencja. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , odczytuje jednostki zostanie usunięty po wyrenderowaniu strony. Gdy `HttpPost` `Edit` metody akcji jest wywoływana, nowe żądania i masz nowe wystąpienie klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), więc trzeba ręcznie ustawić stan jednostki `Modified.` , a następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które można zmienić.
> 
> Jeśli chcesz, aby SQL `Update` instrukcję, aby zaktualizować tylko pola, które użytkownik rzeczywiste zmiany, można zapisać oryginalnych wartości w jakiś sposób (np. ukryte pola), tak aby były dostępne podczas `HttpPost` `Edit` metoda jest wywoływana. Możesz utworzyć `Student` jednostki przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnego obiektu, zaktualizuj wartości jednostki z nowymi wartościami miar, a następnie wywołaj `SaveChanges.` Aby uzyskać więcej informacji, zobacz [ Stany jednostki i SaveChanges](https://msdn.microsoft.com/data/jj592676) i [dane lokalne](https://msdn.microsoft.com/data/jj592872) w Centrum deweloperów danych w witrynie MSDN.


Kod HTML i Razor w *Views\Student\Edit.cshtml* jest podobny do przedstawionego w *Create.cshtml*, i nie są wymagane żadne zmiany.

Uruchomienia strony, wybierając **studentów** kartę, a następnie klikając polecenie **Edytuj** hiperłącze.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Niektóre dane i kliknij przycisk Zmień **Zapisz**. Zobaczysz dane zmienione w stronę indeksu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony Delete

W *Controllers\StudentController.cs*, kod szablonu dla `HttpGet` `Delete` metoda używa `Find` metody do pobierania wybranej `Student` jednostki, jak opisany w `Details` i `Edit` metody. Jednak aby zaimplementować niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` zakończy się niepowodzeniem, pewne funkcje należy dodać do tej metody i jego odpowiedniego widoku.

Widoczny dla aktualizacji i operacje tworzenia, operacje usuwania wymagają dwóch metod akcji. Metoda, która jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który zapewnia możliwość zatwierdzania lub anulować operację usuwania. Jeśli użytkownik zatwierdza, tworzona jest żądaniem POST. Jeśli tak się stanie, `HttpPost` `Delete` metoda jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Następnie dodasz `try-catch` za pomocą bloku `HttpPost` `Delete` metody, aby obsłużyć wszystkie błędy, które mogą wystąpić po zaktualizowaniu bazy danych. Jeśli wystąpi błąd, `HttpPost` `Delete` wywołania metody `HttpGet` `Delete` metody, podając mu parametr, który wskazuje, że wystąpił błąd. `HttpGet Delete` Metoda następnie zostanie ponownie strona potwierdzenia oraz komunikat o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp `HttpGet` `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ten kod akceptuje [opcjonalny parametr](https://msdn.microsoft.com/library/dd264739.aspx) oznacza to, czy metoda została wywołana po awarii, aby zapisać zmiany. Ten parametr jest `false` podczas `HttpGet` `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana `HttpPost` `Delete` metody w odpowiedzi na błąd aktualizacji bazy danych, parametr jest `true` i komunikat o błędzie jest przekazywane do widoku.
2. Zastąp `HttpPost` `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywiste i przechwytującą wszystkie błędy aktualizacji bazy danych.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Ten kod pobiera wybranej jednostki, następnie wywołuje [Usuń](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodę, aby ustawić stan jednostki `Deleted`. Gdy `SaveChanges` nosi SQL `DELETE` polecenia jest generowany. Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`. Utworzony szkielet kodu o nazwie `HttpPost` `Delete` metoda `DeleteConfirmed` zapewnienie `HttpPost` unikatowy podpis metody. (Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyj takiej samej nazwie `HttpPost` i `HttpGet` usunięcie metod.

     Jeśli poprawę wydajności w aplikacji mocno obciążające ma najwyższy priorytet, można uniknąć niepotrzebnych zapytanie SQL, aby pobrać wiersza, zastępując wierszy kodu, które wywołują `Find` i `Remove` metody z następującym kodem:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Ten kod tworzy `Student` jednostki przy użyciu tylko wartość klucza podstawowego, a następnie ustawia stan jednostki `Deleted`. To wszystko, który potrzebuje platformy Entity Framework, aby usunąć jednostkę.

     Jak wspomniano, `HttpGet` `Delete` metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonaniem każdej operacji edycji Utwórz operacji lub innej operacji, które zmieniają dane) tworzy to zagrożenie bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Autor: Stephen Walther.
3. W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między `h2` nagłówek i `h3` nagłówka, jak pokazano w poniższym przykładzie:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Uruchomienia strony, wybierając **studentów** kartę i klikając **Usuń** hiperłącze:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Kliknij przycisk **Usuń**. Nie usunięto uczniów zostanie wyświetlona strona indeksu. (Przedstawimy przykład błędu kodu w akcji obsługi [samouczek współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Zamykanie połączenia z bazą danych

Aby zamknąć połączenia z bazą danych i Zwolnij część zasobów, przechowywane w nich tak szybko, jak to możliwe, należy dysponować wystąpienie kontekstu po wykonaniu tych czynności z nim. Oznacza to, dlaczego utworzony szkielet kodu udostępnia [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metody na końcu `StudentController` klasy w *StudentController.cs*, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Podstawowy `Controller` już klasy implementuje `IDisposable` interfejsu, dlatego ten kod po prostu dodaje zastąpienie `Dispose(bool)` metodę, aby jawnie Usuń wystąpienie kontekstu.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Obsługa transakcji

Domyślnie platforma Entity Framework niejawnie implementuje transakcji. W scenariuszach, w którym zmiany do wielu wierszy lub tabeli, a następnie wywołać `SaveChanges`, platformy Entity Framework automatycznie tworzy się, że wszystkie zmiany powiedzie się lub nie powiedzie się. Jeśli niektóre zmiany są najpierw wykonywane, a następnie błąd występuje, te zmiany są automatycznie przywracane. Zobacz scenariusze, w którym możesz muszą większa kontrola — na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji — [Praca z transakcji](https://msdn.microsoft.com/data/dn456843) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD dla `Student` jednostek. Pomocnicy MVC jest używany do generowania elementów interfejsu użytkownika dla pól danych. Aby uzyskać więcej informacji na temat pomocników MVC zobacz [renderowania pomocników HTML za pomocą formularza](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (strona jest dla platformy MVC 3, ale jest nadal istotne dla MVC 5).

W następnym samouczku będziesz rozwiń funkcje strony indeksu, dodając sortowanie i stronicowanie.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów platformy Entity Framework można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [dalej](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
