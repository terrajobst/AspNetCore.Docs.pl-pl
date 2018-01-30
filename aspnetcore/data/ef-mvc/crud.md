---
title: Platformy ASP.NET Core MVC podstawowych EF - CRUD - 2 10
author: tdykstra
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/crud
ms.openlocfilehash: a7e0d4ff3d57e42dd7e33ffb5f26f2143520be87
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Tworzenia, odczytu, aktualizacji i usuwania - Core EF z samouczek platformy ASP.NET Core MVC (2 10)

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W poprzednich samouczek utworzono aplikację MVC, która przechowuje i wyświetla danych przy użyciu programu Entity Framework i bazy danych LocalDB programu SQL Server. W tym samouczku, należy przejrzeć i dostosować CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) kodu, który będzie szkieletów MVC automatycznie tworzy w kontrolery i widoki.

> [!NOTE] 
> Jest typowym rozwiązaniem implementacji klienta wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem a warstwa dostępu do danych. Aby zachować te samouczki proste i skupiają się na nauczania sposobu korzystania z programu Entity Framework samej siebie, nie używają repozytoriów. Informacje o repozytoria z EF, zobacz [ostatniego samouczku tej serii](advanced.md).

W tym samouczku będziesz pracować z następujących stron sieci web:

![Strona szczegółów dla użytkowników domowych](crud/_static/student-details.png)

![Strona tworzenia dla użytkowników domowych](crud/_static/student-create.png)

![Uczniów edycji strony](crud/_static/student-edit.png)

![Strona usuwania studentów](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Dostosowywanie strony szczegółów

Szkieletu kodu strony indeksu studentów pozostać poza `Enrollments` właściwości, ponieważ kolekcja zawiera tej właściwości. W **szczegóły** strony, będzie wyświetlać zawartość kolekcji, w tabeli HTML.

W *Controllers/StudentsController.cs*, metody akcji, aby uzyskać więcej informacji, Wyświetl używa `SingleOrDefaultAsync` metoda pobierania pojedynczy `Student` jednostki. Dodaj kod, który wywołuje `Include`. `ThenInclude`, a `AsNoTracking` metod, jak pokazano w poniższym kodzie zaznaczony.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

`Include` i `ThenInclude` metody powodują kontekstu ładowania `Student.Enrollments` właściwość nawigacji i w obrębie każdej rejestracji `Enrollment.Course` właściwości nawigacji.  Dowiesz się więcej na temat tych metod w [odczytywanie danych powiązanych](read-related-data.md) samouczka.

`AsNoTracking` Metody umożliwia zwiększenie wydajności w scenariuszach, w którym zwróconych nie zostaną zaktualizowane w okresie istnienia w bieżącym kontekście. Dowiesz się więcej na temat `AsNoTracking` na końcu tego samouczka.

### <a name="route-data"></a>Dane trasy

Wartość klucza, który jest przekazywany do `Details` metody pochodzi z *przekazywanie danych*. Dane trasy to dane integratora modelu znaleziono w segmencie adresu URL. Na przykład trasy domyślnej określa kontroler, akcję i identyfikator segmentów:

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

Następujący adres URL trasy domyślnej mapuje instruktora co kontroler, indeks jako akcja i 1 jako identyfikatora; są to wartości danych trasy.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

Ostatnia część adresu URL ("? courseID = 2021") jest wartość ciągu zapytania. Integrator modelu zostaną również przekazać wartość Identyfikatora do `Details` metody `id` parametru, jeśli przekaż go jako wartość ciągu kwerendy:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

Na stronie indeksu Odnośnik URL są tworzone przez instrukcje elementu pomocniczego znacznika w widoku Razor. W poniższym kodzie Razor `id` parametru odpowiada trasy domyślnej tak `id` jest dodawany do danych trasy.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie poniższy kod HTML po `item.ID` to 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

W poniższym kodzie Razor `studentID` nie jest zgodny z parametrem w trasy domyślnej, jest dodawany jako ciąg zapytania.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Spowoduje to wygenerowanie poniższy kod HTML po `item.ID` to 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Aby uzyskać więcej informacji na temat pomocników tagów, zobacz [pomocników tagów w ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Dodać rejestracji do widoku szczegółów

Otwórz *Views/Students/Details.cshtml*. Każde pole jest wyświetlane przy użyciu `DisplayNameFor` i `DisplayFor` pomocników, jak pokazano w poniższym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Po ostatnim polu i bezpośrednio przed tagiem zamykającym `</dl>` tagów, Dodaj następujący kod, aby wyświetlić listę rejestracji:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.

Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego rejestracji Wyświetla tytuł kursu i kategorii. Tytuł kursu są pobierane z jednostki kursu, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.

Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **szczegóły** łącze studenta. Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów:

![Strona szczegółów dla użytkowników domowych](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

W *StudentsController.cs*, zmodyfikuj HttpPost `Create` metody bloku try-catch Dodawanie i usuwanie identyfikator z `Bind` atrybutu.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Ten kod dodaje jednostki uczniów utworzony przez obiekt wiążący modelu platformy ASP.NET MVC do jednostki studentów ustawić, a następnie zapisuje zmiany w bazie danych. Integrator modelu odwołuje się do funkcji ASP.NET MVC, który ułatwi pracę z danych przesyłanych przez formularz, (integratora modelu konwertuje wartości przesłanego formularza na typy CLR i przekazuje je do metody akcji w parametrach. W tym przypadku integratora modelu tworzy wystąpienie jednostki uczniów przy użyciu wartości właściwości z kolekcji formularza.)

Możesz usunąć `ID` z `Bind` atrybutu, ponieważ identyfikator ma wartość klucza podstawowego, której program SQL Server ustawi automatycznie, gdy zostanie wstawiona. Dane wejściowe użytkownika nie zmienia wartości Identyfikatora.

Inne niż `Bind` atrybutu, blok try-catch jest tylko zmiany wprowadzone do szkieletu kodu. Jeśli wyjątek, która jest pochodną `DbUpdateException` jest zgłoszony, gdy trwa zapisywanie zmian, zostanie wyświetlony komunikat ogólny błąd. `DbUpdateException`wyjątki są czasami spowodowane coś zewnętrzne do aplikacji, a nie błąd programistyczny, dlatego zalecane jest użytkownik, aby spróbować ponownie. Chociaż nie jest zaimplementowana w tym przykładzie, jakości aplikacji produkcyjnej może zarejestruje wyjątek. Aby uzyskać więcej informacji, zobacz **dziennik, aby uzyskać szczegółowe informacje o** sekcji [monitorowanie i dane telemetryczne (tworzenia rzeczywistych aplikacji w chmurze platformy Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

`ValidateAntiForgeryToken` Atrybut zapobiega fałszerstwie żądania międzywitrynowego (CSRF). Token jest automatycznie wprowadzić do widoku przez [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) i jest dostępne po przesłaniu formularza przez użytkownika. Token jest zweryfikowany przez `ValidateAntiForgeryToken` atrybutu. Aby uzyskać więcej informacji o CSRF, zobacz [żądania przed sfałszowaniem](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Uwaga dotycząca zabezpieczeń o overposting

`Bind` Atrybut, który zawiera utworzony szkielet kodu na `Create` metoda jest jednym ze sposobów ochrony przed overposting w tworzenie scenariuszy. Na przykład, załóżmy, że zawiera jednostki uczniów `Secret` właściwości, których nie chcesz, aby ta strona sieci web, aby ustawić.

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

Nawet jeśli nie masz `Secret` pola na stronie sieci web, haker można za pomocą narzędzia, takie jak Fiddler lub zapisać fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość. Bez `Bind` atrybutu ograniczanie pól używanych przez integrator modelu podczas tworzenia wystąpienia dla użytkowników domowych, integratora modelu czy odebrania który `Secret` stanowią wartość i go użyć do utworzenia wystąpienia jednostki studenta. Następnie niezależnie od wartości haker określony dla `Secret` pola formularza, czy zaktualizowane w bazie danych. Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.

![Dodawanie pola tajny fiddler](crud/_static/fiddler.png)

Wartość "OverPost" może następnie pomyślnie dodane do `Secret` właściwości wstawionego wiersza, chociaż nie powinny strony sieci web i można ustawić tej właściwości.

Overposting w scenariuszach edycji można zapobiec przez najpierw odczytu jednostki bazy danych, a następnie wywołując `TryUpdateModel`, przekazując Jawne dozwolonych właściwości listy. To metodę użytą w tych samouczkach.

Alternatywny sposób, aby zapobiec overposting, który jest wybierany przez wielu deweloperów jest użycie modeli widoku zamiast klas jednostek dla wiązania modelu. Zawierać tylko właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu integratora modelu MVC, skopiuj Wyświetl właściwości modelu do tego wystąpienia jednostki, opcjonalnie przy użyciu narzędzia, takiego jak AutoMapper. Użyj `_context.Entry` na wystąpienie jednostki, aby ustawić stanu `Unchanged`, a następnie ustaw `Property("PropertyName").IsModified` na wartość true dla każdej właściwości jednostki, który znajduje się w modelu widoku. Ta metoda działa zarówno w edytować i tworzyć scenariuszy.

### <a name="test-the-create-page"></a>Utwórz strona testowa

Kod w *Views/Students/Create.cshtml* używa `label`, `input`, i `span` (dla komunikatów dotyczących sprawdzania poprawności) pomocników dla każdego pola tagów.

Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **Utwórz nowy**.

Wprowadź nazwy i daty. Spróbuj wprowadzić nieprawidłową datę, jeśli przeglądarki umożliwia to zrobić. (W niektórych przeglądarkach wymusza użyj selektora daty.) Następnie kliknij przycisk **Utwórz** Aby wyświetlić komunikat o błędzie.

![Błąd sprawdzania poprawności daty](crud/_static/date-error.png)

Jest to weryfikacji po stronie serwera, który można pobrać domyślnie; w samouczku nowsze zobaczysz jak dodać atrybuty, które również wygeneruje kod weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu w `Create` metody.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Zmień wartość na prawidłową datę, a następnie kliknij przycisk **Utwórz** aby zobaczyć nowe student są wyświetlane w **indeksu** strony.

## <a name="update-the-edit-page"></a>Aktualizacja edycji strony

W *StudentController.cs*, HttpGet `Edit` — metoda (jeden bez `HttpPost` atrybut) używa `SingleOrDefaultAsync` metoda pobierania wybranej jednostki dla użytkowników domowych, jak przedstawiono w `Details` metody. Nie trzeba będzie zmienić tę metodę.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Zalecane HttpPost edycji kodu: Odczyt i aktualizacja

Zastąp następujący kod metody HttpPost edycji akcji.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Te zmiany implementuje zaleceniami dotyczącymi zabezpieczeń, aby zapobiec overposting. Tworzenia szkieletu, generowane `Bind` atrybutu i dodać obiekt utworzony przez obiekt wiążący modelu do zestawu z jednostek `Modified` flagi. Czy kod nie jest zalecane w różnych scenariuszach, ponieważ `Bind` atrybutu usuwa wszystkie istniejące dane w polach niewymienionych w `Include` parametru.

Nowy kod odczytuje istniejącej jednostki i wywołania `TryUpdateModel` można zaktualizować pola w pobraną jednostkę [oparte na dane wejściowe użytkownika w danych przesłanego formularza](xref:mvc/models/model-binding#how-model-binding-works). Automatyczna zmiana Entity Framework śledzenia zestawy `Modified` flagi w polach, które są zmieniane przez dane wejściowe formularza. Gdy `SaveChanges` metoda jest wywoływana, Entity Framework utworzy instrukcje SQL w celu zaktualizowania wiersza w tabeli bazy danych. Konfliktom współbieżności są ignorowane, a kolumn tabeli, które zostały zaktualizowane przez użytkownika są aktualizowane w bazie danych. (Nowsze samouczek przedstawia sposób obsługi konfliktom współbieżności.)

Najlepszym rozwiązaniem, aby zapobiec overposting pola, które mają być aktualizuje **Edytuj** strony są białej w `TryUpdateModel` parametrów. (Pusty ciąg powyższej listy pól na liście parametrów jest dla prefiksu nazwy pola formularza za pomocą). Obecnie nie ma żadnych dodatkowych pól, które chronisz, ale lista pól, które mają integratora modelu do powiązania gwarantuje, że jeśli dodajesz pola do modelu danych w przyszłości, są one automatycznie chronione aż dodasz je tutaj.

W wyniku tych zmian podpis metody HttpPost `Edit` metody jest taka sama jak HttpGet `Edit` metody; w związku z tym została zmieniona metoda `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Kod HttpPost Edytuj alternatywnych: Utwórz i Dołącz

Zalecane kodu edycji HttpPost gwarantuje, że tylko zmienione kolumny pobierania aktualizacji i zachowuje dane w właściwości, które nie mają być dołączane do wiązania modelu. Jednak odczytu pierwszego podejścia wymaga dodatkowych bazy danych do odczytu i może spowodować bardziej złożonego kodu do obsługi konfliktom współbieżności. Alternatywą jest dołączyć jednostki utworzone przez integratora modelu dla kontekstu EF i oznacz ją jako zmodyfikowane. (Nie Aktualizuj projektu o tym kodzie tylko wykazał ilustrujący opcjonalne podejście.) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Tej metody można użyć po stronie interfejsu użytkownika sieci web obejmuje wszystkie pola w jednostce i można ich zaktualizować.

Kod z utworzonym szkieletem używa metody tworzenia i dołączyć, ale tylko przechwytuje `DbUpdateConcurrencyException` wyjątki i zwraca 404 kody błędów.  Przykład pokazany przechwytuje żadnych wyjątków aktualizacji bazy danych i wyświetla komunikat o błędzie.

### <a name="entity-states"></a>Stany jednostki

Przechowuje informacje o kontekście bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych, a te informacje określa, co się stanie w przypadku wywołania `SaveChanges` metody. Na przykład podczas przekazywania nową jednostkę do `Add` — metoda, która stanu jednostki jest ustawiona na `Added`. Następnie podczas wywoływania `SaveChanges` metody kontekst bazy danych wydaje polecenia SQL INSERT.

Jednostka może być w jednym z następujących stanów:

* `Added`. Jednostka nie istnieje jeszcze w bazie danych. `SaveChanges` Metody wystawia instrukcji INSERT.

* `Unchanged`. Nie trzeba żadnego ustawienia można zrobić za pomocą tej jednostki przez `SaveChanges` metody. Podczas odczytu jednostki bazy danych, jednostka rozpoczyna się od tego stanu.

* `Modified`. Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. `SaveChanges` Metody wystawia instrukcji UPDATE.

* `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metody wystawia instrukcji DELETE.

* `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.

W aplikacji pulpitu zmian stanu zwykle są ustawiane automatycznie. Przeczytaj jednostki i wprowadzić zmiany w niektóre z jej wartości właściwości. Powoduje to, że jego stan jednostki automatycznie zmieniona na `Modified`. Następnie podczas wywoływania `SaveChanges`, Entity Framework generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko rzeczywiste właściwości, które zostało zmienione.

W aplikacji sieci web `DbContext` który początkowo odczytuje jednostki i wyświetla jego dane można edytowane zostanie usunięty po renderowania strony. Gdy HttpPost `Edit` metoda akcji jest wywoływana, nowych żądań sieci web i masz nowe wystąpienie klasy `DbContext`. Jeśli ponownie odczytać jednostki, w tym kontekście nowe, można symulować przetwarzania pulpitu.

Ale jeśli nie chcesz zrobić nadmiarowe operacja odczytu, należy użyć obiektu jednostki, utworzonego przez integratora modelu.  Najprostszym sposobem, aby to zrobić, jest do ustawienia stanu jednostki zmodyfikowane, co jest wykonywane w kodzie HttpPost Edytuj alternatywnych przedstawiona wcześniej. Następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizacji wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które zostało zmienione.

Jeśli chce się uniknąć odczytu pierwszego podejścia, ale ma także instrukcji SQL UPDATE można zaktualizować tylko pola, które użytkownik faktycznie zmienił, kod jest bardziej złożony. Masz próbę zapisania oryginalnych wartości w jakiś sposób (takich jak przy użyciu pola ukryte), dzięki czemu są one dostępne podczas HttpPost `Edit` metoda jest wywoływana. Następnie można utworzyć jednostki dla użytkowników domowych, przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnej jednostki, zaktualizuj wartości jednostki do nowych wartości, a następnie wywołać `SaveChanges`.

### <a name="test-the-edit-page"></a>Testowanie edycji strony

Uruchom aplikację, wybierz **studentów** , a następnie kliknij **Edytuj** hiperłącza.

![Strona Edytuj uczniów lub studentów](crud/_static/student-edit.png)

Niektóre dane i kliknij przycisk Zmień **zapisać**. **Indeksu** spowoduje otwarcie strony, aby zobaczyć zmienione dane.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

W *StudentController.cs*, kod szablonu HttpGet `Delete` używa metody `SingleOrDefaultAsync` metoda pobierania wybranej jednostki dla użytkowników domowych, jak przedstawiono w szczegóły i Edytuj metody. Jednak do zaimplementowania niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` nie powiedzie się, niektóre funkcje zostanie dodana do tej metody i jego odpowiedni widok.

Instrukcji dotyczących aktualizacji i tworzenia operacji operacji usuwania wymaga dwóch metod akcji. Metoda jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulować operację usuwania. Jeśli użytkownik zaakceptuje go, tworzona jest wysłanie żądania POST. Jeśli tak się stanie, HttpPost `Delete` metoda jest wywoływana, a następnie metoda faktycznie wykonuje operację usuwania.

Blok try-catch zostanie dodana do HttpPost `Delete` można obsłużyć wszystkie błędy, które mogą wystąpić, gdy baza danych jest aktualizowana. Jeśli wystąpi błąd, metoda HttpPost Delete wywołuje metodę HttpGet Delete przekazanie jej przez parametr, który wskazuje, że wystąpił błąd. Metody HttpGet Delete zostanie następnie ponownie stronę potwierdzenia oraz komunikat o błędzie, która umożliwia użytkownikowi możliwość anulowania lub spróbuj ponownie.

Zastąp HttpGet `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Ten kod przyjmuje opcjonalny parametr, który wskazuje, czy metoda została wywołana po awarii, aby zapisać zmiany. Ten parametr ma wartość false, gdy HttpGet `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana przez HttpPost `Delete` metody w odpowiedzi na błąd aktualizacji bazy danych, parametr ma wartość true, komunikat o błędzie jest przekazywane do widoku.

### <a name="the-read-first-approach-to-httppost-delete"></a>Podejście pierwszy odczytu do usunięcia HttpPost

Zastąp HttpPost `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywistego oraz przechwytującą wszystkie błędy aktualizacji bazy danych.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Ten kod pobiera wybranej jednostki, następnie wywołuje `Remove` metody w celu ustawienia stanu jednostki `Deleted`. Gdy `SaveChanges` jest nazywany SQL DELETE wygenerowaniu polecenia.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>Utwórz i dołączyć podejście do usunięcia HttpPost

W przypadku zwiększania wydajności aplikacji dużych priorytet, można uniknąć niepotrzebnych kwerend SQL przez utworzenie wystąpienia jednostki dla użytkowników domowych, przy użyciu tylko podstawowy klucz wartość, a następnie ustawienie stanu jednostki `Deleted`. To wszystko, który programu Entity Framework wymaga, aby usunąć jednostkę. (Nie należy umieszczać tego kodu w projekcie; tutaj jest tylko w celu zilustrowania zamiast).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Jeśli jednostka ma powiązane dane, które również zostaną usunięte, upewnij się, że usuwanie kaskadowe jest skonfigurowany w bazie danych. Takie podejście do usuwania jednostki EF może mieli się, że istnieją powiązanych jednostek do usunięcia.

### <a name="update-the-delete-view"></a>Aktualizowanie widoku Delete

W *Views/Student/Delete.cshtml*, Dodaj komunikat o błędzie między nagłówków h2 i nagłówek h3, jak pokazano w poniższym przykładzie:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **usunąć** hiperłącze:

![Usuń stronę potwierdzenia](crud/_static/student-delete.png)

Kliknij przycisk **usunąć**. Bez uczniów usuniętych zostanie wyświetlona strona indeksu. (Będzie Zobacz przykład obsługi kodu w akcji w samouczku współbieżności błędów).

## <a name="closing-database-connections"></a>Zamykanie połączenia bazy danych

Aby zwolnić zasoby, które przechowuje połączenia z bazą danych, wystąpienia kontekstu musi zostać usunięty tak szybko, jak to możliwe po zakończeniu z nim. Wbudowane platformy ASP.NET Core [iniekcji zależności](../../fundamentals/dependency-injection.md) zajmuje to zadanie za użytkownika.

W *Startup.cs*, należy wywołać [— metoda rozszerzenia AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) umożliwia kontrolerowi wyznaczenie `DbContext` klasy w kontenerze Podpisane ASP.NET. Czy metoda Ustawia okres istnienia usługi `Scoped` domyślnie. `Scoped`oznacza, że okres istnienia obiektu kontekstu pokrywa się z czasem życia żądania sieci web, i `Dispose` metoda zostanie wywołana automatycznie na końcu żądania sieci web.

## <a name="handling-transactions"></a>Obsługa transakcji

Domyślnie programu Entity Framework niejawnie implementuje transakcji. W scenariuszach, gdzie zmianę wielu wierszy lub tabel, a następnie wywołać `SaveChanges`, Entity Framework automatycznie upewnia się, że wszystkie zmiany powiedzie się lub nie ich wszystkich. Jeśli niektóre zmiany są najpierw wykonywane, a następnie błąd wystąpi, zmiany te są automatycznie wycofana. Scenariusze, w którym należy więcej kontrolujesz — na przykład, jeśli chcesz dołączyć operacje wykonywane poza Entity Framework w transakcji — można znaleźć [transakcji](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Zapytania dotyczące śledzenia nie

Jeśli kontekst bazy danych pobiera wiersze tabeli i tworzy obiekty jednostki, które reprezentują je, domyślnie go przechowuje informacje o czy jednostek w pamięci są zsynchronizowane z nowości w bazie danych. Dane w pamięci działa jako pamięci podręcznej i jest używany podczas aktualizacji jednostki. Często jest wykorzystywana w aplikacji sieci web to buforowanie, ponieważ kontekst wystąpienia są zwykle krótkotrwałą (nowy jest utworzony i usunięty dla każdego żądania) oraz kontekst które odczytuje jednostki zazwyczaj zostanie usunięty, zanim będzie można ponownie użyć tej jednostki.

Możesz wyłączyć śledzenie obiektów jednostek w pamięci przez wywołanie metody `AsNoTracking` metody. Następujące typowe scenariusze, w których można to zrobić:

* Okres istnienia kontekstu nie trzeba zaktualizować żadnych jednostek i nie trzeba EF do [automatycznie załadować właściwości nawigacji z jednostkami pobierane przez oddzielne zapytania](read-related-data.md). Często te warunki są spełnione w metod akcji kontrolera HttpGet.

* Używasz kwerendę, która pobiera dużej ilości danych, a tylko niewielką część zwrócone dane zostaną zaktualizowane. Może być bardziej efektywne Wyłącz śledzenie dla dużych zapytanie i uruchom zapytanie później dla kilku jednostek, które muszą zostać zaktualizowane.

* Aby dołączyć jednostki, aby można było zaktualizować go, ale wcześniej pobrać tej samej jednostki w innym celu. Ponieważ jednostka jest już śledzony przez kontekst bazy danych, nie można dołączyć jednostki, która ma zostać zmieniony. Jednym ze sposobów obsłużyć taką sytuację jest wywołać `AsNoTracking` na wcześniejsze zapytanie.

Aby uzyskać więcej informacji, zobacz [śledzenia wersji programu vs. Śledzenie nr](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD dla uczniów jednostek. W następnym samouczku będzie rozszerzyć funkcjonalność **indeksu** dodając sortowanie, filtrowanie i stronicowania.

>[!div class="step-by-step"]
[Poprzednie](intro.md)
[dalej](sort-filter-page.md)  
