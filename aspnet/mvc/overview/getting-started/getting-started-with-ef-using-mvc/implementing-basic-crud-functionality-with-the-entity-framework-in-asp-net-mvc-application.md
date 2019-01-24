---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Samouczek: Implementowanie funkcji CRUD z platformą Entity Framework we wzorcu ASP.NET MVC | Dokumentacja firmy Microsoft'
description: Przejrzyj i dostosować tworzenia, odczytywać, aktualizować, usuwania (CRUD) kod tworzący MVC scaffolding automatycznie w widoków i kontrolerów.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 42971b4e15c4aaf85d0ea50bf63934ba4a78ed91
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836237"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Samouczek: Implementowanie funkcji CRUD z platformą Entity Framework we wzorcu ASP.NET MVC

W [poprzedniego samouczka](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), utworzyć aplikację MVC, która przechowuje i wyświetla dane przy użyciu Entity Framework (EF) 6 i SQL Server LocalDB. W ramach tego samouczka możesz przejrzeć i dostosować tworzenia, odczytywać, aktualizować, usuwania (CRUD) kod tworzący MVC scaffolding automatycznie dla Ciebie w widoków i kontrolerów.

> [!NOTE]
> Jest to powszechną praktyką w celu zaimplementowania wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem i warstwy dostępu do danych. Aby zachować te samouczki, prosty i skupiają się na nauczania sposób korzystania z programów EF 6, samego, nie używają repozytoriów. Aby uzyskać informacje o sposobie wdrażania repozytoriów, zobacz [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Poniżej przedstawiono przykłady stron sieci web, które możesz utworzyć:

![Zrzut ekranu strony szczegółów dla uczniów.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Zrzut ekranu przedstawiający student Utwórz stronę.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Zrzut ekranu ot student usuwanie strony.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utwórz stronę szczegółów
> * Utwórz stronę aktualizacji
> * Metoda HttpPost edycji aktualizacji
> * Aktualizuj stronę Delete
> * Połączenia bazy danych zamknij
> * Obsługa transakcji

## <a name="prerequisites"></a>Wymagania wstępne

* [Tworzenie modelu danych Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Utwórz stronę szczegółów

Utworzony szkielet kodu dla uczniów lub studentów `Index` strony pozostawiono `Enrollments` właściwość, ponieważ ta właściwość zawiera kolekcję. W `Details` stronie zawartość kolekcji będą wyświetlane w tabeli HTML.

W *Controllers\StudentController.cs*, metodę akcji dla `Details` wyświetlić używa [znaleźć](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) metodę, aby pobrać jeden `Student` jednostki.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Wartość klucza jest przekazywany do metody jako `id` parametru i pochodzi z *kierować dane* w **szczegóły** hiperłącze na stronę indeksu.

### <a name="tip-route-data"></a>Porada: **Dane trasy**

Przekierowywanie danych to dane, które w segment adresu URL określony w tabeli routingu znaleziono integratora modelu. Na przykład określa domyślną trasę `controller`, `action`, i `id` segmenty:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Następujący adres URL mapuje trasy domyślnej `Instructor` jako `controller`, `Index` jako `action` i 1 `id`; są to wartości danych trasy.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` to wartość ciągu zapytania. Integrator modelu również sprawdzi się w przypadku przekazania `id` jako wartość ciągu zapytania:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Adresy URL są tworzone przez `ActionLink` instrukcji w widoku Razor. W poniższym kodzie `id` parametr odpowiada trasa domyślna, więc `id` jest dodawany do kierowania danych.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

W poniższym kodzie `courseID` nie jest zgodny z parametrem dla trasy domyślne, więc zostanie dodany jako ciąg zapytania.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Aby utworzyć stronę szczegółów

1. Otwórz *Views\Student\Details.cshtml*.

   Każde pole jest wyświetlana przy użyciu `DisplayFor` pomocnika, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Po `EnrollmentDate` pól i bezpośrednio przed tagiem zamykającym `</dl>` tagów, należy dodać wyróżniony kod, aby wyświetlić listę rejestracji, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Jeśli wcięcia kodu o szerokość jest nieprawidłowy, po wklejeniu kodu, naciśnij **Ctrl**+**K**, **Ctrl**+**D** do formatowania go.

    Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego `Enrollment` jednostki we właściwości Wyświetla tytuł kurs i klasy korporacyjnej. Tytuł kurs jest pobierana z `Course` jednostki, która jest przechowywana w `Course` właściwość nawigacji `Enrollments` jednostki. Wszystkie te dane są pobierane z bazy danych automatycznie, gdy jest to konieczne. Innymi słowy używane są powolne ładowanie tutaj. Nie określono *wczesne ładowanie* dla `Courses` właściwość nawigacji, więc rejestracji nie zostały pobrane w jednym zapytaniu, która otrzymała uczniów. Zamiast tego po raz pierwszy próbujesz uzyskać dostęp do `Enrollments` właściwość nawigacji nowe zapytanie jest wysyłane do pobierania danych do bazy danych. Możesz dowiedzieć się więcej o powolne ładowanie i wczesne ładowanie w [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.

3. Otwórz stronę szczegółów, uruchamiając program (**Ctrl**+**F5**), wybierając opcję **studentów** kartę, a następnie klikając **szczegóły** linku, aby użytkownik Alexander Carson. (Jeśli użytkownik naciśnie klawisz **Ctrl**+**F5** podczas *Details.cshtml* plik jest otwarty, wystąpi błąd HTTP 400. Jest to spowodowane programu Visual Studio podejmie próbę uruchomienia na stronie szczegółów, ale go nie można nawiązać kontaktu z link, który określa ucznia, aby wyświetlić. Jeśli tak się stanie, Usuń "Dla uczniów/Details" z adresu URL i spróbuj ponownie, lub zamknij przeglądarkę, kliknij prawym przyciskiem myszy projekt i kliknij przycisk **widoku** > **Pokaż w przeglądarce**.)

    Zobaczysz listę klas i kursom dla wybranych uczniów lub studentów.

4. Zamknij przeglądarkę.

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

1. W *Controllers\StudentController.cs*, Zastąp <xref:System.Web.Mvc.HttpPostAttribute> `Create` metody akcji, używając następującego kodu. Ten kod dodaje `try-catch` bloku i usuwa `ID` z <xref:System.Web.Mvc.BindAttribute> atrybut szkieletu metody:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ten kod dodaje `Student` jednostki utworzone przez integrator modelu programu ASP.NET MVC do `Students` jednostki zestawu, a następnie zapisuje zmiany w bazie danych. *Integrator modelu* odwołuje się do funkcjonalność platformy ASP.NET MVC, która pozwala je łatwiej jest pracować z danych przesyłanych przez formularz; integratora modelu w postaci konwertuje opublikowane wartości na typy CLR i przekazuje je do metody akcji w parametrach. W takim przypadku tworzy wystąpienie integratora modelu `Student` jednostki przy użyciu właściwości wartości z kolekcji `Form` kolekcji.

    Możesz usunąć `ID` z powiązania atrybutu, ponieważ `ID` jest wartość klucza podstawowego, który program SQL Server ustawi automatycznie, gdy zostanie wstawiona. Dane wejściowe od użytkownika nie ustawia `ID` wartość.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Ostrzeżenie o zabezpieczeniach - `ValidateAntiForgeryToken` atrybutów pomaga zapobiec [fałszerstwo żądania międzywitrynowego](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków. Wymaga ona odpowiednią `Html.AntiForgeryToken()` instrukcji w widoku, który pojawi się później.

    `Bind` Atrybut jest jednym ze sposobów, aby zapewnić ochronę przed *polegającymi* w tworzenie scenariuszy. Na przykład, załóżmy, że `Student` zawiera jednostki `Secret` właściwości, które nie mają tej strony sieci web, aby ustawić.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Nawet jeśli nie masz `Secret` pola na stronie sieci web haker może użyć narzędzia takie jak [fiddler](http://fiddler2.com/home), lub napisz fragmentów kodu JavaScript do opublikowania `Secret` stanowią wartość. Bez <xref:System.Web.Mvc.BindAttribute> atrybut ograniczanie pól używanych przez integrator modelu podczas tworzenia `Student` wystąpienia<em>,</em> integratora modelu czy wczytać, `Secret` formę wartość i użyć go do utworzenia `Student` wystąpienie jednostki. Następnie niezależnie od wartości haker, określony dla `Secret` pola formularza zostałaby zaktualizowana w bazie danych. Na poniższej ilustracji przedstawiono program fiddler Dodawanie narzędzie `Secret` pola (z wartością "OverPost") do wartości przesłanego formularza.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Wartość "OverPost" następnie zostaną pomyślnie dodane do `Secret` właściwość wstawionego wiersza, ale nigdy nie ma strony sieci web i można ustawić tę właściwość.

    Zaleca się używać `Include` parametrem `Bind` atrybutu *dozwolonych* pola. Istnieje również możliwość użycia `Exclude` parametr *blacklist* pola, które chcesz wykluczyć. Przyczyna `Include` zapewnia większe bezpieczeństwo jest, że po dodaniu nowej właściwości do jednostki, nowe pole nie jest automatycznie chroniona przez `Exclude` listy.

    Można zapobiec overposting w scenariuszach edycji jest najpierw odczytu jednostki z bazy danych, a następnie wywołując `TryUpdateModel`, przekazując jako jawne dozwolonych właściwości listy tych praktyk. To metodę używaną w tych samouczkach.

    Alternatywny sposób, aby zapobiec overposting, który jest wybierany przez wielu deweloperów jest wyświetlanie modeli, a nie klas jednostek za pomocą wiązania modelu. Zawierają właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu integratora modelu MVC skopiuj Wyświetl właściwości modelu do wystąpienia jednostki, takie jak opcjonalnie za pomocą narzędzia [AutoMapper](http://automapper.org/). Użyj bazy danych. Wpis w jednostce wystąpienia, aby ustawić jej stan Unchanged, a następnie ustaw Property("PropertyName"). IsModified na wartość true dla każdej właściwości jednostki, który znajduje się w modelu widoku. Ta metoda działa, w obu edytowanie i tworzenie scenariuszy.

    Inne niż `Bind` atrybutu `try-catch` blok jest tylko zmiany wprowadzone do utworzony szkielet kodu. Jeśli wyjątek, który pochodzi od klasy <xref:System.Data.DataException> jest przechwycony podczas zmiany są zapisywane, jest wyświetlany ogólny komunikat o błędzie. <xref:System.Data.DataException> wyjątki są czasami spowodowane coś zewnętrznego do aplikacji, a nie błąd programowania, dzięki czemu użytkownik jest zalecane, aby spróbować ponownie. Chociaż nie jest zaimplementowana w tym przykładzie, aplikacji jakości produkcyjnej rejestruje wyjątek. Aby uzyskać więcej informacji, zobacz **dziennik, aby uzyskać szczegółowe informacje o** sekcji [monitorowanie i Telemetria (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kod w *Views\Student\Create.cshtml* jest podobny do przedstawionego w *Details.cshtml*, chyba że `EditorFor` i `ValidationMessageFor` pomocników są używane dla każdego pola, zamiast `DisplayFor`. W tym miejscu jest odpowiedni kod:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* obejmuje również `@Html.AntiForgeryToken()`, który współdziała z `ValidateAntiForgeryToken` atrybutu w kontrolerze w celu zapobieżenia [fałszerstwo żądania międzywitrynowego](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków.

    Żadne zmiany nie są wymagane w *Create.cshtml*.

2. Uruchomienia strony, uruchamiając program, wybierając **studentów** kartę, a następnie klikając **Utwórz nowy**.

3. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** aby zobaczyć komunikat o błędzie.

    Jest to weryfikacji po stronie serwera, który można pobrać domyślnej. Później w samouczku pojawi się, jak dodać atrybuty, które generują kod weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu w **Utwórz** metody.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Zmień datę na prawidłową wartość, a następnie kliknij przycisk **Utwórz** do nowego studenta pojawiają się w temacie **indeksu** strony.

5. Zamknij przeglądarkę.

## <a name="update-httppost-edit-method"></a>Metoda HttpPost edycji aktualizacji

1. Zastąp <xref:System.Web.Mvc.HttpPostAttribute> `Edit` metody akcji, używając następującego kodu:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > W *Controllers\StudentController.cs*, `HttpGet Edit` — metoda (jeden bez `HttpPost` atrybutu) używa `Find` metody do pobierania wybranej `Student` jednostki, jak opisany w `Details`metody. Nie trzeba zmienić tę metodę.

   Te zmiany zaimplementować najlepszym rozwiązaniem w zakresie zabezpieczeń zapobiec [polegającymi](#overpost), generator szkieletu generowane `Bind` atrybutu i dodać jednostki utworzone przez integratora modelu do zestawu z flagą zmodyfikowane jednostek. Czy kod nie jest zalecane, ponieważ `Bind` atrybutu powoduje danych zapisanych w polach, które nie są wymienione w `Include` parametru. W przyszłości, generator szkieletu kontrolera MVC zostanie zaktualizowana tak, aby nie generować `Bind` atrybuty dla metod edycji.

   Nowy kod odczytuje istniejącej jednostki i wywołania <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> można zaktualizować pola z danych wejściowych użytkownika w danych przesłanego formularza. Entity Framework automatyczne śledzenie zestawy zmian [EntityState.Modified](<xref:System.Data.EntityState.Modified>) flagi w jednostce. Gdy [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda jest wywoływana, <xref:System.Data.EntityState.Modified> flaga powoduje, że platforma Entity Framework do tworzenia instrukcji SQL, aby zaktualizować wiersz bazy danych. [Konflikty współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) są ignorowane, a wszystkie kolumny wiersza bazy danych zostaną zaktualizowane, łącznie z tymi, które użytkownik nie został zmieniony. (Dalszych samouczków pokazano, jak obsługa konfliktów współbieżności, a jeśli chcesz tylko poszczególne pola do zaktualizowania w bazie danych, można ustawić jednostki [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) i ustaw poszczególnych pól [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Aby zapobiec overposting, pola, które mają być można aktualizować przez stronę edycji są na liście dozwolonych w `TryUpdateModel` parametrów. Aktualnie nie istnieją żadne dodatkowe pola, które chronisz, ale lista pól, które chcesz integratora modelu do powiązania gwarantuje, że po dodaniu pola do modelu danych w przyszłości, są automatycznie chroniona, dopóki nie dodasz je tutaj.

   W wyniku tych zmian podpis metody metody HttpPost edycji jest taka sama jak metoda edycji HttpGet; w związku z tym po zmianie nazwy metody EditPost.

   > [!TIP]
   >
   > **Stany jednostki i dołączania i metod SaveChanges**
   >
   > Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych, a te informacje określa, co się stanie, gdy wywołujesz `SaveChanges` metody. Na przykład podczas przekazywania do nowej jednostki [Dodaj](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metody, która stan obiektu jest ustawiony na `Added`. Następnie, gdy zostanie wywołana [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody wystawia kontekst bazy danych SQL `INSERT` polecenia.
   >
   > Jednostki może być jedną z następujących [stany](xref:System.Data.EntityState):
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
   > Jeśli chcesz, aby SQL `Update` instrukcję, aby zaktualizować tylko pola, które użytkownik rzeczywiste zmiany, można zapisać oryginalnych wartości w jakiś sposób (np. ukryte pola), tak aby były dostępne podczas `HttpPost` `Edit` metoda jest wywoływana. Możesz utworzyć `Student` jednostki przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnego obiektu, zaktualizuj wartości jednostki z nowymi wartościami miar, a następnie wywołaj `SaveChanges.` Aby uzyskać więcej informacji, zobacz [ Stany jednostki i SaveChanges](/ef/ef6/saving/change-tracking/entity-state) i [dane lokalne](/ef/ef6/querying/local-data).

   Kod HTML i Razor w *Views\Student\Edit.cshtml* jest podobny do przedstawionego w *Create.cshtml*, i nie są wymagane żadne zmiany.

2. Uruchomienia strony, uruchamiając program, wybierając **studentów** kartę, a następnie klikając **Edytuj** hiperłącze.

3. Niektóre dane i kliknij przycisk Zmień **Zapisz**. Zobaczysz dane zmienione w stronę indeksu.

4. Zamknij przeglądarkę.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W *Controllers\StudentController.cs*, kod szablonu dla <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metoda używa `Find` metody do pobierania wybranej `Student` jednostki, jak opisany w `Details` i `Edit` metody. Jednak aby zaimplementować niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` zakończy się niepowodzeniem, pewne funkcje należy dodać do tej metody i jego odpowiedniego widoku.

Widoczny dla aktualizacji i operacje tworzenia, operacje usuwania wymagają dwóch metod akcji. Metoda, która jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który zapewnia możliwość zatwierdzania lub anulować operację usuwania. Jeśli użytkownik zatwierdza, tworzona jest żądaniem POST. Jeśli tak się stanie, `HttpPost` `Delete` metoda jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Następnie dodasz `try-catch` za pomocą bloku <xref:System.Web.Mvc.HttpPostAttribute> `Delete` metody, aby obsłużyć wszystkie błędy, które mogą wystąpić po zaktualizowaniu bazy danych. Jeśli wystąpi błąd, <xref:System.Web.Mvc.HttpPostAttribute> `Delete` wywołania metody <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metody, podając mu parametr, który wskazuje, że wystąpił błąd. <xref:System.Web.Mvc.HttpGetAttribute> `Delete` Metoda następnie zostanie ponownie strona potwierdzenia oraz komunikat o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ten kod akceptuje [opcjonalny parametr](https://msdn.microsoft.com/library/dd264739.aspx) oznacza to, czy metoda została wywołana po awarii, aby zapisać zmiany. Ten parametr jest `false` podczas `HttpGet` `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana `HttpPost` `Delete` metody w odpowiedzi na błąd aktualizacji bazy danych, parametr jest `true` i komunikat o błędzie jest przekazywane do widoku.

2. Zastąp <xref:System.Web.Mvc.HttpPostAttribute> `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywiste i przechwytującą wszystkie błędy aktualizacji bazy danych.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Ten kod pobiera wybranej jednostki, następnie wywołuje [Usuń](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodę, aby ustawić stan jednostki `Deleted`. Gdy `SaveChanges` nosi SQL `DELETE` polecenia jest generowany. Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`. Utworzony szkielet kodu o nazwie `HttpPost` `Delete` metoda `DeleteConfirmed` zapewnienie `HttpPost` unikatowy podpis metody. (Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyj takiej samej nazwie `HttpPost` i `HttpGet` usunięcie metod.

    Jeśli poprawę wydajności w aplikacji mocno obciążające ma najwyższy priorytet, można uniknąć niepotrzebnych zapytanie SQL, aby pobrać wiersza, zastępując wierszy kodu, które wywołują `Find` i `Remove` metody z następującym kodem:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Ten kod tworzy `Student` jednostki przy użyciu tylko wartość klucza podstawowego, a następnie ustawia stan jednostki `Deleted`. To wszystko, który potrzebuje platformy Entity Framework, aby usunąć jednostkę.

    Jak wspomniano, `HttpGet` `Delete` metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonaniem każdej operacji edycji Utwórz operacji lub innej operacji, które zmieniają dane) tworzy to zagrożenie bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Autor: Stephen Walther.

3. W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między `h2` nagłówek i `h3` nagłówka, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Uruchomienia strony, uruchamiając program, wybierając **studentów** kartę, a następnie klikając **Usuń** hiperłącze.

5. Wybierz **Usuń** na stronie, która wynika z **czy na pewno chcesz usunąć ten?**.

    Wyświetla strony indeksu bez usunięto uczniów. (Przedstawimy przykład błędu kodu w akcji obsługi [samouczek współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Połączenia bazy danych zamknij

Aby zamknąć połączenia z bazą danych i Zwolnij część zasobów, przechowywane w nich tak szybko, jak to możliwe, należy dysponować wystąpienie kontekstu po wykonaniu tych czynności z nim. Oznacza to, dlaczego utworzony szkielet kodu udostępnia [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metody na końcu `StudentController` klasy w *StudentController.cs*, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Podstawowy `Controller` już klasy implementuje `IDisposable` interfejsu, dlatego ten kod po prostu dodaje zastąpienie `Dispose(bool)` metodę, aby jawnie Usuń wystąpienie kontekstu.

## <a name="handle-transactions"></a>Obsługa transakcji

Domyślnie platforma Entity Framework niejawnie implementuje transakcji. W scenariuszach, w którym zmiany do wielu wierszy lub tabeli, a następnie wywołać `SaveChanges`, platformy Entity Framework automatycznie tworzy się, że wszystkie zmiany powiedzie się lub nie powiedzie się. Jeśli niektóre zmiany są najpierw wykonywane, a następnie błąd występuje, te zmiany są automatycznie przywracane. W scenariuszach, w których należy użytkownik większa kontrola&mdash;na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji&mdash;zobacz [Praca z transakcji](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Pobierz kod

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Dodatkowe zasoby

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD dla `Student` jednostek. Pomocnicy MVC jest używany do generowania elementów interfejsu użytkownika dla pól danych. Aby uzyskać więcej informacji na temat pomocników MVC, zobacz [renderowania pomocników HTML za pomocą formularza](/previous-versions/aspnet/dd410596(v=vs.98)) (artykuł jest dla platformy MVC 3, ale jest nadal istotne dla MVC 5).

Linki do innych programów EF 6 zasobów można znaleźć w [dostęp do danych platformy ASP.NET — zalecane zasoby](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utworzona strona szczegółów
> * Zaktualizowana strona tworzenia
> * Zaktualizowano metody HttpPost edycji
> * Zaktualizowane strony usuwania
> * Połączenia zamknięte bazy danych
> * Obsługuje transakcje

Przejdź do następnego artykułu, aby dowiedzieć się, jak dodać sortowanie, filtrowanie i stronicowanie do projektu.
> [!div class="nextstepaction"]
> [Sortowanie, filtrowanie i stronicowanie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)