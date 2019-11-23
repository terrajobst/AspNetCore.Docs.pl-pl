---
title: 'Samouczek: Implementowanie funkcji CRUD — ASP.NET MVC z EF Core'
description: W tym samouczku opisano, jak przeglądać i dostosowywać kod CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), który automatycznie tworzy szkielet MVC dla Ciebie w kontrolerach i widokach.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2019
ms.topic: tutorial
uid: data/ef-mvc/crud
ms.openlocfilehash: f0c5bcff4c4b0808f9b4703e1429c3a6d1a7a2d7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259718"
---
# <a name="tutorial-implement-crud-functionality---aspnet-mvc-with-ef-core"></a>Samouczek: Implementowanie funkcji CRUD — ASP.NET MVC z EF Core

W poprzednim samouczku utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu Entity Framework i SQL Server LocalDB. W tym samouczku opisano, jak przeglądać i dostosowywać kod CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie), który automatycznie tworzy szkielet MVC dla Ciebie w kontrolerach i widokach.

> [!NOTE]
> Jest to typowa Metoda implementacji wzorca repozytorium w celu utworzenia warstwy abstrakcji między kontrolerem i warstwą dostępu do danych. Aby zachować te samouczki jako proste i skoncentrowane na tym, jak korzystać z Entity Framework, nie korzystają z repozytoriów. Aby uzyskać informacje o repozytoriach z EF, zobacz [ostatni samouczek w tej serii](advanced.md).

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Dostosuj stronę szczegółów
> * Aktualizowanie strony tworzenia
> * Zaktualizuj strony edytowania
> * Aktualizuj stronę Delete
> * Zamknij połączenia bazy danych

## <a name="prerequisites"></a>Wymagania wstępne

* [Wprowadzenie do EF Core i ASP.NET Core MVC](intro.md)

## <a name="customize-the-details-page"></a>Dostosuj stronę szczegółów

Kod szkieletu dla strony indeksu studentów nie wyprowadził właściwości `Enrollments`, ponieważ ta właściwość zawiera kolekcję. Na stronie **szczegółów** zostanie wyświetlona zawartość kolekcji w tabeli HTML.

W obszarze *controllers/StudentsController. cs*Metoda akcji dla widoku szczegółów używa metody `SingleOrDefaultAsync` do pobrania pojedynczej jednostki `Student`. Dodaj kod, który wywołuje `Include`. `ThenInclude`i metody `AsNoTracking`, jak pokazano w poniższym wyróżnionym kodzie.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

Metody `Include` i `ThenInclude` powodują, że kontekst załaduje właściwość nawigacji `Student.Enrollments` i w ramach każdej rejestracji `Enrollment.Course` właściwość nawigacji.  Dowiesz się więcej na temat tych metod w samouczku [odczytywanie powiązanych danych](read-related-data.md) .

Metoda `AsNoTracking` zwiększa wydajność w scenariuszach, w których zwrócone jednostki nie będą aktualizowane w okresie istnienia bieżącego kontekstu. Dowiesz się więcej na temat `AsNoTracking` na końcu tego samouczka.

### <a name="route-data"></a>Dane trasy

Wartość klucza, która jest przesyłana do metody `Details`, pochodzi z *danych tras*. Dane trasy to dane, które spinacz modelu znalazł w segmencie adresu URL. Na przykład trasa domyślna określa segmenty kontrolera, akcji i identyfikatora:

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

W poniższym adresie URL, domyślna trasa mapuje instruktora jako rolę kontrolera, indeks jako akcję i 1 jako identyfikator; są to wartości danych tras.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

Ostatnią częścią adresu URL ("? courseID = 2021") jest wartość ciągu zapytania. Segregator modelu przekaże także wartość identyfikatora do `Index` metody `id` parametr, Jeśli przekażesz go jako wartość ciągu zapytania:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Na stronie indeks adresy URL hiperłączy są tworzone przez instrukcje pomocnika tagów w widoku Razor. W poniższym kodzie Razor parametr `id` jest zgodny z domyślną trasą, więc `id` jest dodawany do danych trasy.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie następującego kodu HTML, gdy `item.ID` to 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

W poniższym kodzie Razor `studentID` nie jest zgodny z parametrem w domyślnej trasie, więc jest dodawana jako ciąg zapytania.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie następującego kodu HTML, gdy `item.ID` to 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Aby uzyskać więcej informacji na temat pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

### <a name="add-enrollments-to-the-details-view"></a>Dodawanie rejestracji do widoku szczegółów

Otwórz *Widok/studenci/szczegóły. cshtml*. Każde pole jest wyświetlane przy użyciu `DisplayNameFor` i `DisplayFor` pomocników, jak pokazano w następującym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Po ostatnim polu i bezpośrednio przed tagiem zamykającym `</dl>` Dodaj następujący kod, aby wyświetlić listę rejestracji:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Jeśli Wcięcie kodu jest nieprawidłowe po wklejeniu kodu, naciśnij klawisze CTRL-K-D, aby je poprawić.

Ten kod pętle za pomocą jednostek we właściwości nawigacji `Enrollments`. Dla każdej rejestracji jest wyświetlany tytuł kursu i Klasa. Tytuł kursu jest pobierany z jednostki kursu, która jest przechowywana we właściwości nawigacji `Course` jednostki rejestracji.

Uruchom aplikację, wybierz kartę **studenci** i kliknij link **szczegóły** dla ucznia. Zostanie wyświetlona lista kursów i ocen dla wybranego ucznia:

![Strona szczegółów ucznia](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Aktualizowanie strony tworzenia

W *StudentsController.cs*Zmień metodę HTTPPOST `Create`, dodając blok try-catch i usuwając identyfikator z atrybutu `Bind`.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Ten kod dodaje jednostkę ucznia utworzoną przez spinacz modelu ASP.NET Core MVC do zestawu jednostek uczniów, a następnie zapisuje zmiany w bazie danych. (Model spinacza odnosi się do funkcji ASP.NET Core MVC, które ułatwiają pracę z danymi przesyłanymi przez formularz; segregator modelu konwertuje wartości zaksięgowanych formularzy na typy CLR i przekazuje je do metody akcji w parametrach. W takim przypadku spinacz modelu tworzy wystąpienie jednostki ucznia przy użyciu wartości właściwości z kolekcji formularzy.)

`ID` z atrybutu `Bind`, ponieważ identyfikator jest wartością klucza podstawowego, która SQL Server zostanie ustawiona automatycznie podczas wstawiania wiersza. Dane wejściowe użytkownika nie ustawiają wartości identyfikatora.

Poza atrybutem `Bind` blok try-catch jest jedyną zmianą dokonaną w kodzie szkieletowym. Jeśli wyjątek, który pochodzi z `DbUpdateException` jest przechwytywany podczas zapisywania zmian, zostanie wyświetlony ogólny komunikat o błędzie. wyjątki `DbUpdateException` są czasami spowodowane przez coś zewnętrznego dla aplikacji, a nie z powodu błędu programowania, dlatego należy spróbować ponownie. Chociaż nie jest zaimplementowany w tym przykładzie, aplikacja do jakości produkcyjnej mógłby rejestrować wyjątek. Aby uzyskać więcej informacji, zobacz sekcję **log for Insight** w temacie [monitorowanie i telemetrię (Tworzenie aplikacji w chmurze w rzeczywistości na platformie Azure)](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

Atrybut `ValidateAntiForgeryToken` pomaga zapobiegać atakom na żądania między lokacjami (CSRF). Token jest automatycznie wprowadzany do widoku przez [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) i jest dołączany, gdy formularz zostanie przesłany przez użytkownika. Token jest weryfikowany przez atrybut `ValidateAntiForgeryToken`. Aby uzyskać więcej informacji na temat CSRF, zobacz [zapobieganie żądaniu fałszerstwa](../../security/anti-request-forgery.md).

<a id="overpost"></a>

### <a name="security-note-about-overposting"></a>Uwaga dotycząca zabezpieczeń dotyczącej przefinalizowania

Atrybut `Bind`, który obejmuje kod szkieletowy, w metodzie `Create` jest jednym ze sposobów ochrony przed przepełnieniem w ramach tworzenia scenariuszy. Załóżmy na przykład, że jednostka ucznia zawiera właściwość `Secret`, która nie powinna być ustawiona przez tę stronę sieci Web.

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

Nawet jeśli nie masz pola `Secret` na stronie sieci Web, haker może użyć narzędzia takiego jak programu Fiddler lub napisać kod JavaScript, aby ogłosić `Secret` wartość formularza. Bez atrybutu `Bind` ograniczając pola, które są używane przez spinacz modelu podczas tworzenia wystąpienia ucznia, spinacz modelu utworzy ten `Secret` wartość formularza i użyje go do utworzenia wystąpienia jednostki ucznia. Następnie, niezależnie od wartości, haker określony dla pola formularza `Secret` zostanie zaktualizowany w bazie danych. Na poniższej ilustracji przedstawiono Narzędzie programu Fiddler, które dodaje pole `Secret` (z wartością "naddawaj") do wartości zaksięgowanych formularzy.

![Programu Fiddler Dodawanie pola tajnego](crud/_static/fiddler.png)

Wartość "przeznaczenie" zostanie następnie pomyślnie dodana do właściwości `Secret` wstawionego wiersza, mimo że nie ma możliwości ustawienia tej właściwości na stronie sieci Web.

Można zapobiec nadpisywaniu w scenariuszach edycji, odczytując najpierw jednostkę z bazy danych, a następnie wywołując `TryUpdateModel`, przekazując na listę jawnie dozwolonych właściwości. Jest to metoda używana w tych samouczkach.

Alternatywny sposób zapobiegania przechodzeniu, który jest preferowany przez wielu deweloperów, polega na użyciu modeli widoku zamiast klas jednostek z powiązaniem modelu. Uwzględnij tylko właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu tworzenia spinacza modelu MVC Skopiuj właściwości modelu widoku do wystąpienia jednostki, opcjonalnie używając narzędzia takiego jak automapowanie. Użyj `_context.Entry` w wystąpieniu jednostki, aby ustawić jego stan na `Unchanged`, a następnie ustaw `Property("PropertyName").IsModified` na true dla każdej właściwości jednostki, która jest uwzględniona w modelu widoku. Ta metoda działa zarówno w scenariuszach edycji, jak i tworzenia.

### <a name="test-the-create-page"></a>Testowanie strony tworzenia

Kod w *widokach/Students/Create. cshtml* używa `label`, `input`i `span` (dla komunikatów sprawdzania poprawności) pomocników tagów dla każdego pola.

Uruchom aplikację, wybierz kartę **uczniowie** i kliknij pozycję **Utwórz nową**.

Wprowadź nazwy i datę. Spróbuj wprowadzić nieprawidłową datę, jeśli jest to możliwe. (Niektóre przeglądarki wymuszają użycie selektora dat). Następnie kliknij przycisk **Utwórz** , aby wyświetlić komunikat o błędzie.

![Błąd weryfikacji daty](crud/_static/date-error.png)

Jest to weryfikacja po stronie serwera, która jest domyślnie pobierana; w późniejszym samouczku zobaczysz, jak dodać atrybuty, które generują kod dla weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzanie poprawności modelu w metodzie `Create`.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Zmień datę na prawidłową wartość, a następnie kliknij pozycję **Utwórz** , aby zobaczyć, że nowy student zostanie wyświetlony na stronie **indeks** .

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

W *StudentController.cs*, metoda narzędzia HttpGet `Edit` (taka bez atrybutu `HttpPost`) używa metody `SingleOrDefaultAsync` do pobrania wybranej jednostki ucznia, jak pokazano w metodzie `Details`. Nie musisz zmieniać tej metody.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Zalecane HttpPost edycji kodu: Odczyt i aktualizacja

Zastąp metodę Edytuj akcję HttpPost następującym kodem.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Te zmiany implementują najlepsze rozwiązanie w zakresie zabezpieczeń, aby zapobiec przepisywaniu. Szkielet wygenerował atrybut `Bind` i dodał jednostkę utworzoną przez spinacz modelu do zestawu jednostek z flagą `Modified`. Ten kod nie jest zalecany w wielu scenariuszach, ponieważ atrybut `Bind` czyści wszystkie istniejące wcześniej dane w polach, które nie znajdują się w parametrze `Include`.

Nowy kod odczytuje istniejącą jednostkę i wywołuje `TryUpdateModel`, aby aktualizować pola w pobranej jednostce [na podstawie danych wejściowych użytkownika w ogłoszonych formularzach](xref:mvc/models/model-binding). Automatyczne śledzenie zmian Entity Framework ustawia flagę `Modified` dla pól, które są zmieniane przez dane wejściowe formularza. Gdy wywoływana jest metoda `SaveChanges`, Entity Framework tworzy instrukcje SQL, aby zaktualizować wiersz bazy danych. Konflikty współbieżności są ignorowane, a tylko kolumny tabeli, które zostały zaktualizowane przez użytkownika, są aktualizowane w bazie danych. (W dalszej części samouczka pokazano, jak obsłużyć konflikty współbieżności).

Najlepszym rozwiązaniem, aby zapobiec przepisywaniu, pola, które mają być aktualizowane przez stronę **edycji** , są listy dozwolonych w parametrach `TryUpdateModel`. (Pusty ciąg poprzedzający listę pól na liście parametrów jest przeznaczony dla prefiksu do użycia z nazwami pól formularza). Obecnie nie ma żadnych dodatkowych pól, które są chronione, ale lista pól, które mają być powiązane przez spinacz modelu, zapewnia, że jeśli dodasz pola do modelu danych w przyszłości, są one automatycznie chronione, dopóki nie zostaną jawnie dodane do nich w tym miejscu.

W wyniku tych zmian sygnatura metody HttpPost `Edit` jest taka sama jak Metoda narzędzia HttpGet `Edit`; w związku z tym zmieniono nazwę metody `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Alternatywny kod edycji HttpPost: Utwórz i Dołącz

Zalecany kod edycji HttpPost gwarantuje, że tylko zmienione kolumny zostaną zaktualizowane i zachowuje dane we właściwościach, które nie powinny być dołączone do powiązania modelu. Niemniej jednak w przypadku pierwszej procedury odczytu wymagane jest odczytanie dodatkowej bazy danych i może wystąpić bardziej złożony kod służący do obsługi konfliktów współbieżności. Alternatywą jest dołączenie jednostki utworzonej przez spinacz modelu do kontekstu EF i oznaczenie jej jako zmienionej. (Nie Aktualizuj projektu przy użyciu tego kodu, jest on pokazywany tylko w celu zilustrowania opcjonalnego podejścia).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Tego podejścia można użyć, gdy interfejs użytkownika strony sieci Web zawiera wszystkie pola w jednostce i może je zaktualizować.

Kod szkieletu używa metody Create-and-Attach, ale tylko przechwytuje `DbUpdateConcurrencyException` wyjątki i zwraca kody błędów 404.  Pokazany przykład przechwytuje dowolny wyjątek aktualizacji bazy danych i wyświetli komunikat o błędzie.

### <a name="entity-states"></a>Stany jednostek

Kontekst bazy danych śledzi, czy jednostki w pamięci są zsynchronizowane z odpowiadającymi im wierszami w bazie danych, a te informacje określają, co się stanie w przypadku wywołania metody `SaveChanges`. Na przykład w przypadku przekazania nowej jednostki do metody `Add` stan tej jednostki jest ustawiony na `Added`. Następnie wywołując metodę `SaveChanges`, kontekst bazy danych wystawia polecenie SQL INSERT.

Jednostka może być w jednym z następujących stanów:

* `Added`., Jednostka jeszcze nie istnieje w bazie danych. Metoda `SaveChanges` wystawia instrukcję INSERT.

* `Unchanged`., Nie trzeba wykonywać żadnych czynności za pomocą tej jednostki za pomocą metody `SaveChanges`. Po odczytaniu jednostki z bazy danych jednostka zaczyna się od tego stanu.

* `Modified`., Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. Metoda `SaveChanges` wystawia instrukcję UPDATE.

* `Deleted`., Jednostka została oznaczona do usunięcia. Metoda `SaveChanges` wystawia instrukcję DELETE.

* `Detached`., Jednostka nie jest śledzona przez kontekst bazy danych.

W aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. Odczytywanie jednostki i wprowadzanie zmian w niektórych wartościach właściwości. Powoduje to, że stan jednostki jest automatycznie zmieniany na `Modified`. Po wywołaniu `SaveChanges`, Entity Framework generuje instrukcję SQL UPDATE, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.

W aplikacji sieci Web `DbContext`, która początkowo odczytuje jednostkę i wyświetla dane do edycji, jest usuwana po wyrenderowaniu strony. Gdy wywoływana jest metoda akcji HttpPost `Edit`, zostanie wykonane nowe żądanie sieci Web i masz nowe wystąpienie `DbContext`. Jeśli odczytasz jednostkę w tym nowym kontekście, zasymulujesz przetwarzanie na pulpicie.

Ale jeśli nie chcesz wykonywać dodatkowej operacji odczytu, musisz użyć obiektu Entity utworzonego przez spinacz modelu.  Najprostszym sposobem, aby to zrobić, jest ustawienie stanu jednostki na zmodyfikowany jako wykonany w alternatywnym kodzie edycji HttpPost. Po wywołaniu `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma informacji o właściwościach, które zostały zmienione.

Jeśli chcesz uniknąć pierwszego podejścia do odczytu, ale chcesz również, aby instrukcja SQL UPDATE zaktualizował tylko pola, które użytkownik rzeczywiście zmienił, kod jest bardziej skomplikowany. Musisz zapisać oryginalne wartości w jakiś sposób (na przykład przy użyciu ukrytych pól), aby były dostępne po wywołaniu metody HttpPost `Edit`. Następnie można utworzyć jednostkę ucznia przy użyciu oryginalnych wartości, wywołać metodę `Attach` z tą oryginalną wersją jednostki, zaktualizować wartości jednostki do nowych wartości, a następnie wywołać `SaveChanges`.

### <a name="test-the-edit-page"></a>Testowanie strony edycji

Uruchom aplikację, wybierz kartę **uczniowie** , a następnie kliknij hiperlink **Edit** .

![Strona edycji uczniów](crud/_static/student-edit.png)

Zmień niektóre dane i kliknij przycisk **Zapisz**. Zostanie otwarta strona **indeks** i zobaczysz zmienione dane.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W *StudentController.cs*kod szablonu metody narzędzia HttpGet `Delete` używa metody `SingleOrDefaultAsync` do pobrania wybranej jednostki ucznia, jak pokazano w metodach szczegóły i edycja. Jednak w celu zaimplementowania niestandardowego komunikatu o błędzie, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem, należy dodać do tej metody pewne funkcje i odpowiedni widok.

Podczas operacji aktualizowania i tworzenia należy wykonać operacje usuwania, które wymagają dwóch metod akcji. Metoda wywoływana w odpowiedzi na żądanie GET wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulowania operacji usuwania. Jeśli użytkownik zatwierdzi ten element, zostanie utworzone żądanie POST. Gdy tak się stanie, Metoda HttpPost `Delete` jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Dodasz blok try-catch do metody HttpPost `Delete`, aby obsłużyć wszelkie błędy, które mogą wystąpić podczas aktualizacji bazy danych. Jeśli wystąpi błąd, Metoda Delete HttpPost wywołuje metodę Delete narzędzia HttpGet, przekazując jej parametr wskazujący, że wystąpił błąd. Metoda Delete narzędzia HttpGet następnie ponownie wyświetla stronę potwierdzenia wraz z komunikatem o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

Zastąp metodę akcji narzędzia HttpGet `Delete` następującym kodem, który zarządza raportowaniem błędów.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Ten kod akceptuje opcjonalny parametr, który wskazuje, czy metoda została wywołana po niepowodzeniu zapisania zmian. Ten parametr ma wartość false, gdy metoda `Delete` narzędzia HttpGet jest wywoływana bez wcześniejszego błędu. Gdy jest wywoływana przez metodę `Delete` HttpPost w odpowiedzi na błąd aktualizacji bazy danych, parametr ma wartość true, a komunikat o błędzie jest przesyłany do widoku.

### <a name="the-read-first-approach-to-httppost-delete"></a>Podejście po raz pierwszy do HttpPost usuwania

Zastąp metodę akcji HttpPost `Delete` (o nazwie `DeleteConfirmed`) następującym kodem, który wykonuje rzeczywistą operację usuwania i przechwytuje wszystkie błędy aktualizacji bazy danych.

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6-9,11-12,16-21)]

Ten kod pobiera wybraną jednostkę, a następnie wywołuje metodę `Remove` w celu ustawienia stanu jednostki na `Deleted`. Po wywołaniu `SaveChanges` zostanie wygenerowane polecenie SQL DELETE.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Podejście Create-and-Attach do HttpPost Delete

Jeśli zwiększenie wydajności aplikacji o wysokim poziomie jest priorytetem, można uniknąć niepotrzebnego zapytania SQL przez utworzenie wystąpienia jednostki studenta przy użyciu tylko wartości klucza podstawowego, a następnie ustawienie stanu jednostki na `Deleted`. To wszystko, co Entity Framework potrzebuje, aby usunąć jednostkę. (Nie umieszczaj tego kodu w projekcie; jest to tutaj tylko ilustrujące alternatywę).

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Jeśli jednostka ma powiązane dane, które również należy usunąć, upewnij się, że w bazie danych jest skonfigurowane usuwanie kaskadowe. W ramach tego podejścia do usuwania jednostek EF może nie być możliwe, że istnieją powiązane jednostki, które mają zostać usunięte.

### <a name="update-the-delete-view"></a>Aktualizowanie widoku usuwania

W obszarze *widoki/student/Delete. cshtml*Dodaj komunikat o błędzie między nagłówkiem H2 i nagłówkiem H3, jak pokazano w następującym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uruchom aplikację, wybierz kartę **uczniowie** i kliknij hiperłącze **Usuń** :

![Usuń stronę potwierdzenia](crud/_static/student-delete.png)

Kliknij przycisk **Usuń**. Strona indeks zostanie wyświetlona bez usuniętego ucznia. (W samouczku współbieżności zostanie wyświetlony przykładowy kod obsługi błędu).

## <a name="close-database-connections"></a>Zamknij połączenia bazy danych

Aby zwolnić zasoby, które są przechowywane przez połączenie z bazą danych, wystąpienie kontekstu musi zostać usunięte najszybciej, jak to możliwe, gdy wszystko będzie gotowe. ASP.NET Core wbudowane [iniekcja zależności](../../fundamentals/dependency-injection.md) zajmuje się tym zadaniem.

W *Startup.cs*, należy wywołać [metodę rozszerzenia AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) , aby zainicjować obsługę klasy `DbContext` w ASP.NET Core di kontenera. Ta metoda ustawia okres istnienia usługi do `Scoped` domyślnie. `Scoped` oznacza, że okres istnienia obiektu kontekstu pokrywa się z czasem trwania żądania sieci Web, a metoda `Dispose` zostanie wywołana automatycznie na końcu żądania sieci Web.

## <a name="handle-transactions"></a>Obsługa transakcji

Domyślnie Entity Framework niejawnie implementują transakcje. W scenariuszach, w których wprowadzasz zmiany w wielu wierszach lub tabelach, a następnie Wywołaj `SaveChanges`, Entity Framework automatycznie upewnia się, że wszystkie zmiany zostały wykonane pomyślnie lub wszystkie z nich kończą się niepowodzeniem. Jeśli najpierw zostaną wykonane pewne zmiany, a następnie wystąpi błąd, te zmiany są automatycznie wycofywane. W przypadku scenariuszy, w których potrzebna jest większa kontrola — na przykład jeśli chcesz uwzględnić operacje wykonywane poza Entity Framework w transakcji — zobacz [transakcje](/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Zapytania bez śledzenia

Gdy kontekst bazy danych pobiera wiersze tabeli i tworzy obiekty jednostek, które reprezentują je, domyślnie śledzi, czy jednostki w pamięci są zsynchronizowane z danymi znajdującymi się w bazie danych. Dane w pamięci pełnią rolę pamięci podręcznej i są używane podczas aktualizowania jednostki. Ta pamięć podręczna jest często niepotrzebna w aplikacji sieci Web, ponieważ wystąpienia kontekstu są zwykle krótkotrwałe (nowy element jest tworzony i usuwany dla każdego żądania), a kontekst, który odczytuje jednostkę, jest zazwyczaj likwidowany przed ponownym użyciem tej jednostki.

Można wyłączyć śledzenie obiektów jednostki w pamięci, wywołując metodę `AsNoTracking`. Typowe scenariusze, w których warto wykonać następujące czynności:

* W okresie istnienia kontekstu nie trzeba aktualizować żadnych jednostek, a program EF nie musi [automatycznie ładować właściwości nawigacji z jednostkami pobranymi przez osobne zapytania](read-related-data.md). Często te warunki są spełnione w metodach działania narzędzia HttpGet kontrolera.

* Używasz zapytania pobierającego dużą ilość danych, a tylko niewielka część zwróconych danych zostanie zaktualizowana. Może być bardziej wydajne, aby wyłączyć śledzenie dla dużego zapytania i uruchomić zapytanie później dla kilku jednostek, które wymagają aktualizacji.

* Chcesz dołączyć jednostkę, aby ją zaktualizować, ale wcześniej pobrano tę samą jednostkę do innego celu. Ponieważ jednostka jest już śledzona przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniona. Jednym ze sposobów obsługi tej sytuacji jest wywołanie `AsNoTracking` na wcześniejszym zapytaniu.

Aby uzyskać więcej informacji, zobacz [śledzenie a nie śledzenie](/ef/core/querying/tracking).

## <a name="get-the-code"></a>Pobierz kod

[Pobierz lub Wyświetl ukończoną aplikację.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono następujące instrukcje:

> [!div class="checklist"]
> * Dostosowana Strona szczegółów
> * Zaktualizowano stronę tworzenia
> * Zaktualizowano stronę edycji
> * Zaktualizowano stronę usuwania
> * Zamknięte połączenia bazy danych

Przejdź do następnego samouczka, aby dowiedzieć się, jak rozszerzyć funkcjonalność strony **indeksu** , dodając sortowanie, filtrowanie i stronicowanie.

> [!div class="nextstepaction"]
> [Następne: sortowanie, filtrowanie i stronicowanie](sort-filter-page.md)
