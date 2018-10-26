---
title: Platforma ASP.NET Core MVC z programem EF Core — CRUD - 2 z 10
author: rick-anderson
description: ''
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/crud
ms.openlocfilehash: 34927415beadaa3f5c9035a9101e3c99f7cbc395
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090826"
---
# <a name="aspnet-core-mvc-with-ef-core---crud---2-of-10"></a>Platforma ASP.NET Core MVC z programem EF Core — CRUD - 2 z 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).

W poprzednim samouczku utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu platformy Entity Framework i programu SQL Server LocalDB. W tym samouczku będziesz przejrzenie i dostosowanie CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) kod, który MVC scaffolding automatycznie utworzy dla Ciebie w widoków i kontrolerów.

> [!NOTE]
> Jest to powszechną praktyką w celu zaimplementowania wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem i warstwy dostępu do danych. Aby zachować te samouczki, prosty i skupiają się na nauczania, jak używać programu Entity Framework sam, nie używają repozytoriów. Informacje dla repozytoriów ze EF, zobacz [ostatni samouczek z tej serii](advanced.md).

W tym samouczku będziesz pracować z następujących stron sieci web:

![Strona szczegółów dla uczniów](crud/_static/student-details.png)

![Strona tworzenia dla uczniów](crud/_static/student-create.png)

![Strona edytowania dla uczniów](crud/_static/student-edit.png)

![Strona usuwania dla uczniów](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Dostosowywanie strony szczegółów

Utworzony szkielet kodu strony indeksu studentów pozostawiono `Enrollments` właściwość, ponieważ ta właściwość zawiera kolekcję. W **szczegóły** stronie zawartość kolekcji będą wyświetlane w tabeli HTML.

W *Controllers/StudentsController.cs*, metody akcji, aby uzyskać szczegółowe informacje, Wyświetl używa `SingleOrDefaultAsync` metodę, aby pobrać jeden `Student` jednostki. Dodaj kod, który wywołuje `Include`. `ThenInclude`, a `AsNoTracking` metody, jak pokazano w poniższym kodzie wyróżnione.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` i `ThenInclude` metody powodują kontekst można załadować `Student.Enrollments` właściwość nawigacji i w ramach każdej rejestracji `Enrollment.Course` właściwości nawigacji.  Dowiesz się więcej na temat tych metod w [odczytywanie powiązanych danych](read-related-data.md) samouczka.

`AsNoTracking` Metody zwiększa wydajność w scenariuszach, gdzie zwróconych nie zostanie zaktualizowana w okresie istnienia w bieżącym kontekście. Dowiesz się więcej na temat `AsNoTracking` na końcu tego samouczka.

### <a name="route-data"></a>Dane trasy

Wartość klucza, który jest przekazywany do `Details` metody pochodzi z *kierować dane*. Przekierowywanie danych to dane, które integratora modelu znaleziono w segmencie adresu URL. Na przykład trasy domyślnej określa kontroler, akcję i identyfikator segmenty:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Następujący adres URL trasy domyślnej mapuje przez instruktorów jako kontroler, indeks jako akcję i 1 jako identyfikatora; są to wartości danych trasy.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

Ostatnia część adresu URL ("? courseID = 2021") to wartość ciągu zapytania. Integrator modelu będzie również przekazać wartość Identyfikatora `Details` metoda `id` parametru, jeśli przekazujesz ją jako wartość ciągu zapytania:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Na stronie indeksu adresy URL hiperłącza są tworzone przez instrukcje elementu pomocniczego znacznika w widoku Razor. W poniższym kodzie Razor `id` parametr odpowiada trasa domyślna, więc `id` jest dodawany do kierowania danych.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie poniższy kod HTML po `item.ID` wynosi 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

W poniższym kodzie Razor `studentID` nie jest zgodny z parametrem dla trasy domyślne, więc zostanie dodany jako ciąg zapytania.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie poniższy kod HTML po `item.ID` wynosi 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Aby uzyskać więcej informacji na temat pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

### <a name="add-enrollments-to-the-details-view"></a>Dodawanie rejestracji do widoku szczegółów

Otwórz *Views/Students/Details.cshtml*. Każde pole jest wyświetlana przy użyciu `DisplayNameFor` i `DisplayFor` pomocników, jak pokazano w poniższym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Po ostatnim polu, a także bezpośrednio przed tagiem zamykającym `</dl>` tag, Dodaj następujący kod, aby wyświetlić listę rejestracji:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

W przypadku wcięcia kodu o szerokość problem po wklejeniu kodu, naciśnij kombinację klawiszy CTRL-K-D go poprawić.

Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Do każdej rejestracji Wyświetla tytuł kursu i klasy korporacyjnej. Tytuł kursu jest pobierana z kursu jednostki, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.

Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **szczegóły** link dla uczniów lub studentów. Zostanie wyświetlona lista kursów i ocen dla wybranych uczniów lub studentów:

![Strona szczegółów dla uczniów](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

W *StudentsController.cs*, zmodyfikuj HttpPost `Create` metoda przez dodanie bloku try / catch i usunięcie Identyfikatora z `Bind` atrybutu.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Ten kod dodaje jednostki dla uczniów, utworzone przez integrator modelu programu ASP.NET Core MVC do jednostki studentów Ustaw, a następnie zapisuje zmiany w bazie danych. (Integratora modelu, który odwołuje się do funkcji programu ASP.NET Core MVC ułatwia pracę z danych przesyłanych przez formularz; integratora modelu konwertuje wartości przesłanego formularza na typy CLR i przekazuje je do metody akcji w parametrach. W tym przypadku integratora modelu tworzy wystąpienie jednostki dla uczniów przy użyciu wartości właściwości z kolekcji formularza.)

Możesz usunąć `ID` z `Bind` atrybutu, ponieważ identyfikator ma wartość klucza podstawowego, który program SQL Server ustawi automatycznie, gdy zostanie wstawiona. Dane wejściowe od użytkownika nie należy ustawić wartość Identyfikatora.

Inne niż `Bind` , bloku try / catch jest tylko zmiany wprowadzone do utworzony szkielet kodu. Jeśli wyjątek, który pochodzi od klasy `DbUpdateException` jest przechwycony podczas zmiany są zapisywane, jest wyświetlany ogólny komunikat o błędzie. `DbUpdateException` wyjątki są czasami spowodowane coś zewnętrznego do aplikacji, a nie błąd programowania, dzięki czemu użytkownik jest zalecane, aby spróbować ponownie. Chociaż nie jest zaimplementowana w tym przykładzie, aplikacji jakości produkcyjnej rejestruje wyjątek. Aby uzyskać więcej informacji, zobacz **dziennik, aby uzyskać szczegółowe informacje o** sekcji [monitorowanie i Telemetria (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

`ValidateAntiForgeryToken` Atrybut zapobiega fałszerstwo żądania międzywitrynowego (CSRF) ataków. Token są automatycznie wstrzykiwane do widoku przez [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) i jest dostępne po przesłaniu formularza przez użytkownika. Token jest sprawdzana `ValidateAntiForgeryToken` atrybutu. Aby uzyskać więcej informacji na temat CSRF zobacz [ochrona przed fałszerstwem żądań](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Uwaga dotycząca zabezpieczeń dotyczące overposting

`Bind` Atrybut, który zawiera utworzony szkielet kodu, na `Create` metodą jest jednym ze sposobów, aby zapewnić ochronę przed overposting w tworzenie scenariuszy. Załóżmy, że zawiera jednostki dla uczniów `Secret` właściwości, które nie mają tej strony sieci web, aby ustawić.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Nawet jeśli nie masz `Secret` pola na stronie sieci web haker może za pomocą narzędzia takiego jak Fiddler lub zapisać fragmentów kodu JavaScript do opublikowania `Secret` stanowią wartość. Bez `Bind` atrybut ograniczanie pól używanych przez integrator modelu podczas tworzenia wystąpienia dla uczniów, integratora modelu czy wczytać, `Secret` formę wartość i użyć go do utworzenia wystąpienia jednostki dla uczniów. Następnie niezależnie od wartości haker, określony dla `Secret` pola formularza zostałaby zaktualizowana w bazie danych. Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (z wartością "OverPost") do wartości przesłanego formularza.

![Dodawanie pola wpisu tajnego programu fiddler](crud/_static/fiddler.png)

Wartość "OverPost" następnie zostaną pomyślnie dodane do `Secret` właściwość wstawionego wiersza, ale nigdy nie ma strony sieci web i można ustawić tę właściwość.

Można zapobiec overposting w scenariuszach edycji przez najpierw odczytu jednostki z bazy danych, a następnie wywołując `TryUpdateModel`, przekazując jako jawne dozwolonych właściwości listy tych praktyk. To metodę używaną w tych samouczkach.

Alternatywny sposób, aby zapobiec overposting, który jest wybierany przez wielu deweloperów jest wyświetlanie modeli, a nie klas jednostek za pomocą wiązania modelu. Zawierają właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu integratora modelu MVC skopiuj właściwości modelu widoku wystąpienia jednostki, opcjonalnie za pomocą narzędzia takiego jak AutoMapper. Użyj `_context.Entry` na wystąpienia jednostki, aby ustawić jej stan `Unchanged`, a następnie ustaw `Property("PropertyName").IsModified` na wartość true dla każdej właściwości jednostki, który znajduje się w modelu widoku. Ta metoda działa, w obu edytowanie i tworzenie scenariuszy.

### <a name="test-the-create-page"></a>Strona tworzenia testów

Kod w *Views/Students/Create.cshtml* używa `label`, `input`, i `span` (w przypadku komunikatów dotyczących sprawdzania poprawności) pomocników dla każdego pola tagów.

Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **Utwórz nowy**.

Wprowadź nazwy i daty. Spróbuj wprowadzić nieprawidłową datę, jeśli umożliwia przeglądarce, możesz to zrobić. (Niektóre przeglądarki wymusza użycie selektora daty.) Następnie kliknij przycisk **Utwórz** aby zobaczyć komunikat o błędzie.

![Błąd sprawdzania poprawności daty](crud/_static/date-error.png)

Jest to sprawdzanie poprawności po stronie serwera, które można pobrać domyślnej; później w samouczku pokazano, jak dodać atrybuty, które również spowoduje to wygenerowanie kodu do weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu w `Create` metody.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Zmień datę na prawidłową wartość, a następnie kliknij przycisk **Utwórz** do nowego studenta pojawiają się w temacie **indeksu** strony.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

W *StudentController.cs*, HttpGet `Edit` — metoda (jeden bez `HttpPost` atrybutu) używa `SingleOrDefaultAsync` metody do pobierania wybranej jednostki dla uczniów, jak przedstawiono w `Details` metody. Nie trzeba zmienić tę metodę.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Zaleca się HttpPost edycji kodu: odczytu i aktualizacji

Zastąp metodę akcji edycji HttpPost następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Te zmiany zaimplementować najlepszym rozwiązaniem w zakresie zabezpieczeń zapobiec overposting. Generator szkieletu generowane `Bind` atrybutu i dodać jednostki utworzone przez integratora modelu do zestawu przy użyciu jednostek `Modified` flagi. Czy kod nie jest zalecane dla wielu scenariuszy, ponieważ `Bind` atrybutu powoduje danych zapisanych w polach, które nie są wymienione w `Include` parametru.

Nowy kod odczytuje istniejącej jednostki i wywołania `TryUpdateModel` można zaktualizować pola w pobraną jednostkę [oparte na danych wejściowych użytkownika w danych przesłanego formularza](xref:mvc/models/model-binding#how-model-binding-works). Entity Framework automatyczne śledzenie zestawy zmian `Modified` flagi w polach, które zostaną zmienione przez dane wejściowe formularza. Gdy `SaveChanges` metoda jest wywoływana, platformy Entity Framework tworzy instrukcji SQL, aby zaktualizować wiersz bazy danych. Konflikty współbieżności są ignorowane, a kolumny tabeli, które zostały zaktualizowane przez użytkownika są aktualizowane w bazie danych. (Dalszych samouczków pokazano, jak obsługa konfliktów współbieżności).

Najlepszym rozwiązaniem, aby zapobiec polegającymi pola, które mają być można aktualizować za **Edytuj** strony są na liście dozwolonych w `TryUpdateModel` parametrów. (Jest pusty ciąg poprzedzający listę pól na liście parametrów dla prefiksu nazwy pola formularza za pomocą.) Aktualnie nie istnieją żadne dodatkowe pola, które chronisz, ale lista pól, które chcesz integratora modelu do powiązania gwarantuje, że po dodaniu pola do modelu danych w przyszłości, są automatycznie chroniona, dopóki nie dodasz je tutaj.

W wyniku tych zmian, podpis metody HttpPost `Edit` metody jest taka sama jak HttpGet `Edit` metody; w związku z tym po zmianie nazwy metody `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternatywne HttpPost edycji kodu: tworzenie i dołączanie

Zalecane kodu Edytuj HttpPost gwarantuje, że tylko zmienione kolumny zostaje zaktualizowana i zachowuje dane we właściwościach, które nie mają włączone do wiązania modelu. Podejście pierwszy odczytu wymaga jednak dodatkowe bazy danych odczytu i może doprowadzić do bardziej skomplikowanym kodzie konfliktów współbieżności. Alternatywą jest dołączyć jednostkę utworzone przez integratora modelu dla kontekstu EF i oznaczyć go jako zmodyfikowane. (Nie zaktualizujesz projekt przy użyciu tego kodu tylko wykazał aby zilustrować podejście opcjonalne.)

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Tej metody można użyć po interfejsie użytkownika strony sieci web zawiera wszystkie pola w jednostce i można ich zaktualizować.

Korzysta z metody create i — dołączanie utworzony szkielet kodu, ale tylko przechwytuje `DbUpdateConcurrencyException` wyjątków i zwraca 404 kody błędów.  Przykład pokazany przechwytuje wszystkie wyjątki aktualizacji bazy danych i wyświetla komunikat o błędzie.

### <a name="entity-states"></a>Stany jednostki

Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych, a te informacje określa, co się stanie, gdy wywołujesz `SaveChanges` metody. Na przykład podczas przekazywania do nowej jednostki `Add` metody, która stan obiektu jest ustawiony na `Added`. Następnie, gdy zostanie wywołana `SaveChanges` metody kontekst bazy danych wydaje polecenie SQL INSERT.

Jednostki mogą być w jednym z następujących stanów:

* `Added`. Jednostka jeszcze nie istnieje w bazie danych. `SaveChanges` Metoda generuje instrukcji INSERT.

* `Unchanged`. Niczego nie trzeba wykonać za pomocą tej jednostki przy `SaveChanges` metody. Podczas odczytywania jednostki z bazy danych, jednostka rozpoczyna się w tym stanie.

* `Modified`. Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. `SaveChanges` Metoda generuje instrukcji UPDATE.

* `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metoda generuje instrukcję DELETE.

* `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.

W przypadku aplikacji komputerowych zmiany stanu są zazwyczaj ustawiane automatycznie. Przeczytaj jednostki i wprowadzić zmiany do niektórych wartości właściwości. Powoduje to, że jego stan jednostki automatycznie był zmieniany na `Modified`. Następnie, gdy zostanie wywołana `SaveChanges`, platformy Entity Framework generuje instrukcji SQL UPDATE, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.

W aplikacji sieci web `DbContext` który początkowo odczytuje jednostki i wyświetla jego danych do edycji zostanie usunięty po wyrenderowaniu strony. Gdy HttpPost `Edit` metody akcji jest wywoływana, nowe żądania sieci web i masz nowe wystąpienie klasy `DbContext`. Jeśli ponownie odczytywana jednostki, w tym kontekście nowych, można symulować przetwarzania pulpitu.

Ale jeśli chcesz zrobić operacja odczytu nadmiarowe, należy użyć obiektu jednostki, utworzone przez integratora modelu.  Jest najprostszym sposobem, w tym celu można ustawić stanu jednostki zmodyfikowane, co jest wykonywane na w alternatywnej HttpPost edycji kodu pokazanego wcześniej. Następnie, gdy zostanie wywołana `SaveChanges`, platformy Entity Framework aktualizacji wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które można zmienić.

Jeśli chcesz uniknąć podejścia odczytu pierwszego, ale ma również instrukcji SQL UPDATE można zaktualizować tylko pola, które użytkownik faktycznie zmienił, kod jest bardziej złożone. Musisz zapisać oryginalne wartości w jakiś sposób (takie jak za pomocą ukrytych pól), dzięki czemu są one dostępne po HttpPost `Edit` metoda jest wywoływana. Możesz utworzyć jednostki dla uczniów, przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnego obiektu, zaktualizuj wartości jednostki z nowymi wartościami miar, a następnie wywołaj `SaveChanges`.

### <a name="test-the-edit-page"></a>Testowanie strony edytowania

Uruchom aplikację, wybierz **studentów** , a następnie kliknij **Edytuj** hiperłącze.

![Strona edytowania uczniów](crud/_static/student-edit.png)

Niektóre dane i kliknij przycisk Zmień **Zapisz**. **Indeksu** zostanie otwarta strona i zobaczyć zmienione dane.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W *StudentController.cs*, kod szablonu HttpGet `Delete` metoda używa `SingleOrDefaultAsync` metoda pobierania wybranej jednostki dla uczniów, jak przedstawiono w szczegółowe informacje i Edytuj metody. Jednak aby zaimplementować niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` zakończy się niepowodzeniem, pewne funkcje należy dodać do tej metody i jego odpowiedniego widoku.

Widoczny dla aktualizacji i operacje tworzenia, operacje usuwania wymagają dwóch metod akcji. Metoda, która jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który zapewnia możliwość zatwierdzania lub anulować operację usuwania. Jeśli użytkownik zatwierdza, tworzona jest żądaniem POST. Gdy tak się stanie, HttpPost `Delete` metoda jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Dodasz bloku try / catch do HttpPost `Delete` metody, aby obsłużyć wszystkie błędy, które mogą wystąpić po zaktualizowaniu bazy danych. Jeśli wystąpi błąd, metoda HttpPost usuwania wywołuje metodę HttpGet Delete, podając mu parametr, który wskazuje, że wystąpił błąd. Metody HttpGet Delete zostanie następnie ponownie strona potwierdzenia oraz komunikat o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

Zastąp HttpGet `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Ten kod przyjmuje opcjonalny parametr, który wskazuje, czy metoda została wywołana po awarii, aby zapisać zmiany. Ten parametr ma wartość false, gdy HttpGet `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana przez HttpPost `Delete` metody w odpowiedzi na błąd aktualizacji bazy danych, parametr ma wartość true, a komunikat o błędzie jest przekazywana do widoku.

### <a name="the-read-first-approach-to-httppost-delete"></a>Pierwszy odczytu sposobem usuwania HttpPost

Zastąp HttpPost `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywiste i przechwytującą wszystkie błędy aktualizacji bazy danych.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Ten kod pobiera wybranej jednostki, następnie wywołuje `Remove` metodę, aby ustawić stan jednostki `Deleted`. Gdy `SaveChanges` nosi SQL DELETE polecenia jest generowany.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Utwórz i — dołączanie podejście do usunięcia HttpPost

Jeśli poprawę wydajności w aplikacji mocno obciążające ma najwyższy priorytet, można uniknąć niepotrzebnych kwerend SQL przez utworzenie wystąpienia jednostki dla uczniów, przy użyciu tylko podstawowy klucz wartość, a następnie ustawienie stanu jednostki `Deleted`. To wszystko, który potrzebuje platformy Entity Framework, aby usunąć jednostkę. (Nie należy umieszczać ten kod w projekcie; tutaj jest tylko w celu zilustrowania alternatywę).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Jeśli jednostka ma powiązanych danych, które również zostaną usunięte, upewnij się, że usuwanie kaskadowe jest skonfigurowany w bazie danych. W przypadku tej metody do usuwania jednostki EF może nie należy pamiętać, że nie ma powiązanej jednostki do usunięcia.

### <a name="update-the-delete-view"></a>Aktualizacja widoku Delete

W *Views/Student/Delete.cshtml*, Dodaj komunikat o błędzie między nagłówka H2 spowoduje i nagłówek h3, jak pokazano w poniższym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **Usuń** hiperłącze:

![Usuń stronę potwierdzenia](crud/_static/student-delete.png)

Kliknij przycisk **Usuń**. Nie usunięto uczniów zostanie wyświetlona strona indeksu. (Będziesz Zobacz przykład kodu w akcji, w tym samouczku współbieżności obsługi błędów).

## <a name="closing-database-connections"></a>Zamykanie połączenia z bazą danych

Aby zwolnić zasoby, które zawiera połączenia z bazą danych, wystąpienie kontekstu musi zostać usunięty możliwie najszybciej po zakończeniu z nim. Wbudowane platformy ASP.NET Core [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) zajmie to zadanie dla Ciebie.

W *Startup.cs*, należy wywołać [— metoda rozszerzenia AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) aprowizację `DbContext` klasy w kontenerze platformy ASP.NET Core DI. Czy metoda Ustawia okres istnienia usługi `Scoped` domyślnie. `Scoped` oznacza, że okres istnienia obiektu kontekstu pokrywa się z czasem życia żądania sieci web, a `Dispose` metoda zostanie wywołana automatycznie na końcu żądania sieci web.

## <a name="handling-transactions"></a>Obsługa transakcji

Domyślnie platforma Entity Framework niejawnie implementuje transakcji. W scenariuszach, w którym zmiany do wielu wierszy lub tabeli, a następnie wywołać `SaveChanges`, platformy Entity Framework automatycznie tworzy się, że wszystkie zmiany powiedzie się lub nie ich wszystkich. Jeśli niektóre zmiany są najpierw wykonywane, a następnie błąd występuje, te zmiany są automatycznie przywracane. Zobacz scenariusze, w którym możesz muszą większa kontrola — na przykład, jeśli chcesz dołączyć operacje wykonywane poza programem Entity Framework w ramach transakcji — [transakcji](/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Bez śledzenia zapytań

Jeśli kontekst bazy danych pobiera wiersze z tabeli, tworzy obiekty jednostki, reprezentujących ich domyślnie go przechowuje informacje o czy jednostki w pamięci są zsynchronizowane z tym, co w bazie danych. Dane w pamięci działa jak pamięć podręczna i jest używany podczas aktualizowania jednostki. Często jest wykorzystywana w aplikacji sieci web tej buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałe (nowy jeden jest tworzony i usuwane dla każdego żądania) i obiekt context, odczytuje jednostki zazwyczaj jest usuwane, zanim ponownie używane jest tej jednostki.

Możesz wyłączyć śledzenie obiekty obiektów w pamięci przez wywołanie metody `AsNoTracking` metody. Następujące typowe scenariusze, w których warto to zrobić:

* W okresie istnienia w kontekście nie trzeba zaktualizować wszelkie jednostek, które nie potrzebują EF do [automatycznie obciążenia właściwości nawigacji z jednostkami pobierane przez oddzielne zapytania](read-related-data.md). Często te warunki są spełnione w metody akcji HttpGet kontrolera.

* Używasz kwerendę, która pobiera dużej ilości danych, a zostanie zaktualizowana tylko niewielki fragment wartości zwracanych danych. Może być bardziej efektywne, aby wyłączyć śledzenie dla dużych zapytania i uruchomić zapytanie później uzyskać kilka jednostek, które muszą zostać zaktualizowane.

* Aby dołączyć jednostkę, aby można było zaktualizować go, ale wcześniej pobrane z tej samej jednostki do różnych celów. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów, aby obsłużyć taką sytuację jest wywołanie `AsNoTracking` na wcześniejsze zapytanie względem.

Aby uzyskać więcej informacji, zobacz [śledzenia programu vs. Bez śledzenia](/ef/core/querying/tracking).

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD dla jednostek dla uczniów. W następnym samouczku będziesz rozszerzyć funkcjonalność **indeksu** stronie przez dodanie sortowanie, filtrowanie i stronicowanie.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](intro.md)
> [dalej](sort-filter-page.md)
