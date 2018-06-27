---
title: Stron razor podstawowych EF w platformy ASP.NET Core - CRUD - 2 8
author: rick-anderson
description: Przedstawia sposób tworzenia, odczytu, aktualizowanie i usuwanie EF podstawowych
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 157257d10306ded3456cd66c186a82edf0ba5d65
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961324"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Stron razor podstawowych EF w platformy ASP.NET Core - CRUD - 2 8

::: moniker range="= aspnetcore-2.0"
Wersja platformy ASP.NET Core 2.0 tego samouczka można znaleźć w [plik PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/PDF-6-18-18.pdf).
::: moniker-end
::: moniker range=">= aspnetcore-2.1"

Przez [Dykstra Tomasz](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

W tym samouczku CRUD szkieletu (tworzenia, odczytu, aktualizowanie i usuwanie) kodu jest przejrzeć i dostosować.

Aby zminimalizować złożoność i zachować te samouczki koncentruje się na podstawowych EF, EF Core kod jest używany w modelach strony. Niektórzy deweloperzy Użyj wzorca usługi warstwy lub repozytorium w, aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwa dostępu do danych.

W w tym samouczku, tworzenia, edycji, usuwania i szczegóły stron Razor w *uczniowie* folderu są sprawdzane.

Kod z utworzonym szkieletem korzysta ze wzorca następujące tworzenie, edytowanie i usuwanie stron:

* Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`.
* Zapisz zmiany w danych z metodą HTTP POST `OnPostAsync`.

Strony indeksu i szczegóły Pobierz i Wyświetl żądanych danych z metodą HTTP GET `OnGetAsync`

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync vs FirstOrDefaultAsync

Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), która jest zazwyczaj preferowany nad [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

 `FirstOrDefaultAsync` jest bardziej efektywne niż `SingleOrDefaultAsync` na jedną jednostkę pobierania:

* Jeśli kod należy sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.
* `SingleOrDefaultAsync` pobiera dane i niepotrzebne działa.
* `SingleOrDefaultAsync` zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.
* `FirstOrDefaultAsync` nie zgłoszenia, jeśli istnieje więcej niż jednej jednostki, która pasuje do części filtru.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

W bardzo szkieletu kodu [metoda FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) można użyć zamiast `FirstOrDefaultAsync`.

`FindAsync`:

* Znajduje obiekt z (klucz podstawowy) klucz podstawowy. Jeśli jednostka entity z atrybutem na PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.
* To proste i zwięzłe.
* Jest zoptymalizowany do odszukania pojedynczej jednostki.
* Może korzystnie wydajności w niektórych sytuacjach, ale rzadko pochodzą do gry dla aplikacji sieci web typowych.
* Niejawnie wykorzystuje [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Ale jeśli chcesz `Include` inne jednostki, następnie `FindAsync` nie jest właściwe. Oznacza to, że może być konieczne porzucenie `FindAsync` i przejdź do zapytania w trakcie Twojej aplikacji.

## <a name="customize-the-details-page"></a>Dostosowywanie strony szczegółów

Przejdź do `Pages/Students` strony. **Edytuj**, **szczegóły**, i **usunąć** łącza są generowane przez [pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/uczniów lub studentów / Index.cshtml* pliku.

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Uruchom aplikację i wybierz **szczegóły** łącza. Adres URL ma postać `http://localhost:5000/Students/Details?id=2`. Identyfikator uczniów jest przekazywany za pomocą ciągu zapytania (`?id=2`).

Zaktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy. Zmiany w dyrektywie page dla każdego z tych stron z `@page` do `@page "{id:int}"`.

Żądanie do strony przy użyciu szablonu trasy "{identyfikator: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404. Na przykład `http://localhost:5000/Students/Details` zwraca błąd 404. Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Uruchom aplikację, kliknij łącze Szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).

Nie zmieniaj globalnie `@page` do `@page "{id:int}"`, sposób podziału tak łącza do strony głównej i tworzenie stron.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Dodawanie danych powiązanych

Nie zawiera utworzony szkielet kodu strony indeksu studentów `Enrollments` właściwości. W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.

`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metoda pobierania pojedynczy `Student` jednostki. Dodaj następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) metody powodują kontekstu ładowania `Student.Enrollments` właściwość nawigacji i w obrębie każdej rejestracji `Enrollment.Course` właściwości nawigacji. Te metody są sprawdzane szczegółowo w samouczku dane dotyczące odczytu.

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) — metoda zwiększa wydajność w scenariuszach, gdy zwracany jednostek nie są aktualizowane w bieżącym kontekście. `AsNoTracking` omówione w dalszej części tego samouczka.

### <a name="display-related-enrollments-on-the-details-page"></a>Na stronie Szczegóły są wyświetlane powiązane rejestracji

Otwórz *Pages/Students/Details.cshtml*. Dodaj następujący wyróżniony kod, aby wyświetlić listę rejestracji:

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.

Poprzedni kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego rejestracji Wyświetla tytuł kursu i kategorii. Tytuł kursu są pobierane z jednostki kursu, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.

Uruchom aplikację, wybierz **studentów** , a następnie kliknij pozycję **szczegóły** łącze studenta. Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów.

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

Aktualizacja `OnPostAsync` metody w *Pages/Students/Create.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Sprawdź [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

W powyższym kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwości w [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). `TryUpdateModelAsync` tylko aktualizuje właściwości wymienionych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

W poprzednim przykładzie:

* Drugi argument (` "student", // Prefix`) to prefiks używany do odszukania wartości. Nie jest uwzględniana wielkość liter.
* Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [modelu powiązania](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Overposting

Przy użyciu `TryUpdateModel` można zaktualizować pola z wartościami oczekujących na opublikowanie jest ze względów bezpieczeństwa, ponieważ nie dopuszcza overposting. Na przykład, załóżmy, że zawiera jednostki uczniów `Secret` właściwości, które ta strona sieci web nie należy zaktualizować lub dodać:

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Nawet jeśli aplikacja nie ma `Secret` można ustawić za pomocą pola Utwórz/Aktualizuj Razor strony, haker `Secret` przez overposting wartość. Haker można użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość. Oryginalny kod nie ograniczają pola, które podczas tworzenia wystąpienia uczniów używa integratora modelu.

Niezależnie od wartości haker określony dla `Secret` pola formularza jest zaktualizowane w bazie danych. Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.

![Dodawanie pola tajny fiddler](../ef-mvc/crud/_static/fiddler.png)

Wartość "OverPost" został pomyślnie dodany do `Secret` właściwości wstawionego wiersza. Projektant aplikacji nie są przeznaczone `Secret` ustawionej z tworzenia strony właściwości.

<a name="vm"></a>

### <a name="view-model"></a>Model widoku

Model widoku zwykle zawiera podzbiór właściwości zawarte w modelu używanego przez aplikację. Model aplikacji jest często nazywana modelu domeny. Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednie jednostkę w bazie danych. Model widoku zawiera tylko właściwości, które są potrzebne dla warstwy interfejsu użytkownika (na przykład tworzenia strony). Oprócz modelu widoku niektóre aplikacje za pomocą powiązania modelu lub modelu wejściowych przekazywania danych między klasy modelu stron Razor strony i przeglądarki. Należy rozważyć `Student` model widoku:

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Wyświetl modele umożliwiają alternatywny sposób, aby zapobiec overposting. Model widoku zawiera tylko właściwości, aby wyświetlić (wyświetlanie) lub zaktualizować.

Poniższy kod używa `StudentVM` modelu widoku, aby utworzyć nowy uczniów:

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu. `SetValues` używa dopasowania nazwy właściwości. Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi mieć właściwości, które pasują.

Przy użyciu `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) zaktualizować w celu zastosowania `StudentVM` zamiast `Student`.

Na stronach Razor `PageModel` klasy pochodnej jest model widoku.

## <a name="update-the-edit-page"></a>Aktualizacja edycji strony

Aktualizuj model strony dla strony edycji. Istotne zmiany są wyróżnione:

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Zmiany kodu są podobne do tworzenia strony kilka wyjątków:

* `OnPostAsync` ma opcjonalny `id` parametru.
* Bieżący uczniów jest pobierana z bazy danych, zamiast tworzenia uczniów puste.
* `FirstOrDefaultAsync` zostało zastąpione [metoda FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). `FindAsync` jest to dobry wybór, wybierając jednostkę z klucza podstawowego. Zobacz [metoda FindAsync](#FindAsync) Aby uzyskać więcej informacji.

### <a name="test-the-edit-and-create-pages"></a>Testowanie edytować i tworzyć strony

Tworzenie i edytowanie kilku jednostek studenta.

## <a name="entity-states"></a>Stany jednostki

Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych. Informacje o synchronizacji kontekst bazy danych określa, co się stanie po [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana. Na przykład, gdy nowy obiekt jest przekazywana do [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) — metoda, która stanu jednostki jest ustawiona na [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Gdy `SaveChangesAsync` jest nazywany bazę danych kontekstu wydaje polecenia SQL INSERT.

Jednostka może działać w jednym z [następujące stany](/dotnet/api/microsoft.entityframeworkcore.entitystate):

* `Added`: Jednostka jeszcze nie istnieje w bazie danych. `SaveChanges` Metody wystawia instrukcji INSERT.

* `Unchanged`: Brak zmian muszą być zapisane przy użyciu tej jednostki. Jednostka ma taki stan, gdy jest do odczytu z bazy danych.

* `Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. `SaveChanges` Metody wystawia instrukcji UPDATE.

* `Deleted`: Jednostka została oznaczona do usunięcia. `SaveChanges` Metody wystawia instrukcji DELETE.

* `Detached`: Jednostka nie jest śledzony przez kontekst bazy danych.

W aplikacji komputerowej zmian stanu zwykle są ustawiane automatycznie. Jednostka jest do odczytu, wprowadzono zmiany i stan jednostki automatycznie zmieniona na `Modified`. Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.

W aplikacji sieci web `DbContext` które odczytuje jednostki i wyświetla dane zostanie usunięty po renderowania strony. Jeśli na stronie `OnPostAsync` metoda jest wywoływana, nowych żądań sieci web oraz nowe wystąpienie klasy `DbContext`. Ponowne czytanie jednostki, w tym kontekście nowe symuluje przetwarzania pulpitu.

## <a name="update-the-delete-page"></a>Zaktualizuj strony usuwania

W tej sekcji kod zostanie dodany do wdrożenia błędu niestandardowego komunikatu, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem. Dodaj ciąg zawierający komunikatów o błędach:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Zastąp `OnGetAsync` metodę z następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Poprzedni kod zawiera parametr opcjonalny `saveChangesError`. `saveChangesError` Wskazuje, czy metoda została wywołana po awarii można usunąć obiektu studenta. Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią. Błędy sieci są bardziej prawdopodobne w chmurze. `saveChangesError`ma wartość false, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika. Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usunięcia nie powiodła się), `saveChangesError` parametr ma wartość true.

### <a name="the-delete-pages-onpostasync-method"></a>Metody Delete OnPostAsync stron

Zastąp `OnPostAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Poprzedni kod pobiera wybranej jednostki, następnie wywołuje [Usuń](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) metody w celu ustawienia stanu jednostki `Deleted`. Gdy `SaveChanges` jest nazywany SQL DELETE wygenerowaniu polecenia. Jeśli `Remove` nie powiedzie się:

* Zostanie przechwycony wyjątek bazy danych.
* Usuń strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Zaktualizuj strony Razor usuwania

Dodaj komunikat o błędzie wyróżnione do strony Usuń Razor.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Przetestuj Delete.

## <a name="common-errors"></a>Typowe błędy

Uczniów domowych lub inne łącza nie działa:

Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy. Na przykład, należy na stronie aparatu Razor uczniów domowych **nie** zawiera szablon trasy:

```cshtml
@page "{id:int}"
```

Musi zawierać każdej stronie aparatu Razor `@page` dyrektywy.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/intro)
> [dalej](xref:data/ef-rp/sort-filter-page)
