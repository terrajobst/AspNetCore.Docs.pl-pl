---
title: Strony razor z programem EF Core w programie ASP.NET Core — CRUD - 2, 8
author: rick-anderson
description: Pokazuje, jak tworzyć, odczytywać, aktualizować, usuwać z programem EF Core
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 31fefc148040d7b65e9b65d6bf19f502ef9de542
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795567"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — CRUD - 2, 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Tom Dykstra](https://github.com/tdykstra), [Jan Kowalski P](https://twitter.com/thereformedprog), i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

W tym samouczku szkieletu CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) przejrzeniu i dostosowywać kod.

Aby zminimalizować złożoność, zachowując tych samouczków skupia się na programu EF Core, kod programem EF Core jest używany w modelach strony. Niektórzy deweloperzy Użyj warstwy usług lub [wzorca repozytorium](xref:fundamentals/repository-pattern) Aby utworzyć warstwę abstrakcji między interfejsu użytkownika (Razor strony) i warstwy dostępu do danych.

W tym samouczku, Utwórz, Edytuj, Usuń i szczegóły stron Razor w *uczniów* są sprawdzane w folderze.

Tworzenie, edytowanie i usuwanie stron utworzony szkielet kodu korzysta z następującego wzorca:

* Pobieranie i wyświetlanie żądanych danych za pomocą metody HTTP GET `OnGetAsync`.
* Zapisz zmiany w danych za pomocą metody POST protokołu HTTP `OnPostAsync`.

Strony indeksu i szczegóły get i wyświetlić żądanych danych z metodą HTTP GET `OnGetAsync`

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync programu vs. FirstOrDefaultAsync

Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), która jest ogólnie metoda preferowana nad [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

 `FirstOrDefaultAsync` jest bardziej wydajne niż `SingleOrDefaultAsync` na pobranie jednej jednostki:

* Jeśli kod wymaga sprawdzić, czy nie ma więcej niż jednej jednostki zwróconych przez kwerendę.
* `SingleOrDefaultAsync` Pobieranie większej ilości danych, a niepotrzebne działa.
* `SingleOrDefaultAsync` zgłasza wyjątek, jeśli istnieje więcej niż jednej jednostce, która pasuje do część filtru.
* `FirstOrDefaultAsync` nie wyrzuca, jeśli istnieje więcej niż jednej jednostce, która pasuje do część filtru.

<a name="FindAsync"></a>

### <a name="findasync"></a>FindAsync

W bardzo utworzony szkielet kodu [metoda findasync dla](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) mogą być używane zamiast `FirstOrDefaultAsync`.

`FindAsync`:

* Umożliwia znalezienie jednostki za pomocą kluczowi podstawowemu (PK). Jeśli jednostki z PK jest śledzony przez kontekst, jest zwracany bez żądania z bazą danych.
* Jest proste i zwięzłe.
* Jest zoptymalizowana pod kątem wyszukiwania pojedynczej jednostki.
* Może korzystnie wpływać na wydajności w niektórych sytuacjach, ale są rzadko się dzieje w przypadku aplikacji sieci web typowy.
* Niejawnie wykorzystuje [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

Ale jeśli chcesz `Include` innych podmiotów, następnie `FindAsync` nie jest właściwe. Oznacza to, że może być konieczne porzucenie `FindAsync` i Zacznij korzystać z zapytania w miarę postępów Twojej aplikacji.

## <a name="customize-the-details-page"></a>Dostosowywanie strony szczegółów

Przejdź do `Pages/Students` strony. **Edytuj**, **szczegóły**, i **Usuń** łącza są generowane przez [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w *stron/studentów / Index.cshtml* pliku.

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Uruchom aplikację i wybierz **szczegóły** łącza. Adres URL ma postać `http://localhost:5000/Students/Details?id=2`. Identyfikator dla uczniów jest przekazywany przy użyciu ciągu zapytania (`?id=2`).

Aktualizuj edycji, szczegóły i usuwanie stron Razor, aby użyć `"{id:int}"` szablon trasy. Zmiany strony dyrektyw dla każdej z tych stron z `@page` do `@page "{id:int}"`.

Żądanie strony za pomocą szablonu trasy "{id: int}", który wykonuje **nie** obejmują całkowitą trasy wartość zwraca HTTP (nie znaleziono) błąd 404. Na przykład `http://localhost:5000/Students/Details` zwróci błąd 404. Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Uruchom aplikację, kliknij łącze, szczegóły i sprawdź adres URL jest przekazanie identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).

Nie należy zmieniać globalnie `@page` do `@page "{id:int}"`, wykonując podział tak łącza do strony głównej i tworzenie stron.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Dodawanie danych powiązanych

Nie zawiera utworzony szkielet kodu strony indeksu studentów `Enrollments` właściwości. W tej sekcji zawartości `Enrollments` Kolekcja zostanie wyświetlona na stronie szczegółów.

`OnGetAsync` Metody *Pages/Students/Details.cshtml.cs* używa `FirstOrDefaultAsync` metodę, aby pobrać jeden `Student` jednostki. Dodaj następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) metody powodują kontekst można załadować `Student.Enrollments` właściwość nawigacji i w ramach każdej rejestracji `Enrollment.Course` właściwości nawigacji. Te metody są badane szczegółowo w tym samouczku dane dotyczące odczytu.

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) metody zwiększa wydajność w scenariuszach, gdy jednostki nie są aktualizowane w bieżącym kontekście. `AsNoTracking` omówiono w dalszej części tego samouczka.

### <a name="display-related-enrollments-on-the-details-page"></a>Wyświetl powiązane rejestracje na stronie szczegółów

Otwórz *Pages/Students/Details.cshtml*. Dodaj następujący wyróżniony kod do wyświetlania listy rejestracji:

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

W przypadku wcięcia kodu o szerokość problem po wklejeniu kodu, naciśnij kombinację klawiszy CTRL-K-D go poprawić.

Powyższy kod pętli jednostek w `Enrollments` właściwości nawigacji. Do każdej rejestracji Wyświetla tytuł kursu i klasy korporacyjnej. Tytuł kursu jest pobierana z kursu jednostki, która jest przechowywana w `Course` właściwości nawigacji jednostki rejestracji.

Uruchom aplikację, wybierz **studentów** kartę, a następnie kliknij przycisk **szczegóły** link dla uczniów lub studentów. Zostanie wyświetlona lista kursów i ocen dla wybranych uczniów lub studentów.

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

Aktualizacja `OnPostAsync` method in Class metoda *Pages/Students/Create.cshtml.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Sprawdź [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) kodu:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

W poprzednim kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować `emptyStudent` przy użyciu wartości przesłanego formularza z [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) właściwość [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). `TryUpdateModelAsync` tylko zaktualizowanie właściwości podanych (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

W poprzednim przykładzie:

* Drugi argument (`"student", // Prefix`) jest prefiksem używa do wyszukiwania wartości. Nie jest uwzględniana wielkość liter.
* Wartości przesłanego formularza są konwertowane na typy w `Student` modelu przy użyciu [wiązanie modelu](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>

### <a name="overposting"></a>Polegającymi

Za pomocą `TryUpdateModel` można zaktualizować pola z wartościami przesłanych jest ze względów bezpieczeństwa, ponieważ zapobiega overposting. Załóżmy, że zawiera jednostki dla uczniów `Secret` właściwość, która nie należy zaktualizować tę stronę sieci web, czy dodać:

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Nawet jeśli aplikacja nie ma `Secret` można ustawić pole na stronę Razor, haker tworzenia/aktualizacji `Secret` przez polegającymi wartość. Haker może użyć narzędzia, takiego jak Fiddler lub zapisu fragmentów kodu JavaScript do opublikowania `Secret` stanowią wartość. Oryginalny kod nie są ograniczone pola, które podczas tworzenia wystąpienia dla uczniów używa integratora modelu.

Niezależnie od wartości haker, określony dla `Secret` pola formularza jest zaktualizowana w bazie danych. Na poniższej ilustracji przedstawiono dodawanie narzędzie Fiddler `Secret` pola (z wartością "OverPost") do wartości przesłanego formularza.

![Dodawanie pola wpisu tajnego programu fiddler](../ef-mvc/crud/_static/fiddler.png)

Wartość "OverPost" został pomyślnie dodany do `Secret` właściwość wstawionego wiersza. Projektant aplikacji nie są przeznaczone `Secret` właściwość można ustawić za pomocą tworzenia strony.

<a name="vm"></a>

### <a name="view-model"></a>Model widoku

Model widoku zazwyczaj zawiera podzbiór właściwości zawarte w modelu, używanych przez aplikację. Model aplikacji jest często określany mianem modelu domeny. Model domeny zwykle zawiera wszystkie właściwości, które są wymagane przez odpowiednie jednostki w bazie danych. Model widoku zawiera tylko właściwości potrzebne dla warstwy interfejsu użytkownika (na przykład, Utwórz stronę). Oprócz modelu widoku niektóre aplikacje używają do przesyłania danych między klasy modelu strony stron Razor i przeglądarka powiązanie modelu lub model danych wejściowych. Należy wziąć pod uwagę następujące `Student` model widoku:

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Wyświetl modele zapewniają alternatywny sposób, aby zapobiec overposting. Model widoku zawiera tylko właściwości można wyświetlić (wyświetlanie) ani zaktualizować.

Poniższy kod używa `StudentVM` model widoku, aby utworzyć nowego studenta:

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) metody ustawia wartości tego obiektu, odczytując wartości z innej [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) obiektu. `SetValues` używa Dopasowywanie nazw właściwości. Typ modelu widoku nie musi być powiązany typ modelu, po prostu musi ona mieć właściwości, które odpowiadają.

Za pomocą `StudentVM` wymaga [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) zaktualizowana w celu używania `StudentVM` zamiast `Student`.

W przypadku stron Razor `PageModel` klasy pochodnej jest model widoku.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

Aktualizowanie modelu strony dla strony edytowania. Istotne zmiany są wyróżnione:

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Zmiany kodu są podobne do tworzenia strony z pewnymi wyjątkami:

* `OnPostAsync` ma opcjonalny `id` parametru.
* Bieżący ucznia jest pobierana z bazy danych, zamiast tworzenia puste dla uczniów.
* `FirstOrDefaultAsync` został zastąpiony [metoda findasync dla](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). `FindAsync` jest to dobry wybór, podczas wybierania jednostki z klucza podstawowego. Zobacz [metoda findasync dla](#FindAsync) Aby uzyskać więcej informacji.

### <a name="test-the-edit-and-create-pages"></a>Testowanie edycji i tworzenie stron

Tworzenie i edytowanie kilka jednostek dla uczniów.

## <a name="entity-states"></a>Stany jednostki

Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych. Informacje o synchronizacji w kontekście bazy danych określa, co się stanie, gdy [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) jest wywoływana. Na przykład, gdy nowa jednostka jest przekazywany do [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) metody, która stan obiektu jest ustawiony na [dodano](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Gdy `SaveChangesAsync` jest wywoływana, bazy danych kontekstu wydaje polecenie SQL INSERT.

Jednostki mogą być w jednym z [następujące stany](/dotnet/api/microsoft.entityframeworkcore.entitystate):

* `Added`: Jednostki jeszcze nie istnieje w bazie danych. `SaveChanges` Metoda generuje instrukcji INSERT.

* `Unchanged`: Bez zmian konieczne zapisane przy użyciu tej jednostki. Określona jednostka ma ten stan, gdy jest do odczytu z bazy danych.

* `Modified`: Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. `SaveChanges` Metoda generuje instrukcji UPDATE.

* `Deleted`: Jednostki został oznaczony do usunięcia. `SaveChanges` Metoda generuje instrukcję DELETE.

* `Detached`: Jednostki nie jest śledzony przez kontekst bazy danych.

W przypadku aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. Jednostki jest do odczytu, zostaną wprowadzone zmiany i stanu jednostki automatycznie był zmieniany na `Modified`. Wywoływanie `SaveChanges` generuje instrukcję aktualizacji programu SQL, która aktualizuje tylko zmienionych właściwości.

W aplikacji sieci web `DbContext` który odczytuje jednostki i wyświetla dane, zostanie usunięty po wyrenderowaniu strony. Gdy strona `OnPostAsync` metoda jest wywoływana, nowe żądania sieci web i nowe wystąpienie klasy `DbContext`. Ponowne czytanie jednostki, w tym kontekście nowych symuluje pulpitu przetwarzania.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W tej sekcji kodu zostanie dodany do zaimplementowania błąd niestandardowy komunikat, gdy wywołanie `SaveChanges` zakończy się niepowodzeniem. Dodaj ciąg ma zawierać komunikatów o błędach:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Zastąp `OnGetAsync` metoda następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Powyższy kod zawiera opcjonalny parametr `saveChangesError`. `saveChangesError` Wskazuje, czy metoda została wywołana po nieudanej próbie usunięcia obiektu dla uczniów. Operacja usuwania może zakończyć się niepowodzeniem ze względu na przejściowe problemy z siecią. Błędy przejściowe problemy z siecią jest bardziej prawdopodobne w chmurze. `saveChangesError`ma wartość FAŁSZ, gdy strona usuwania `OnGetAsync` jest wywoływana z interfejsu użytkownika. Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usuwania nie powiodła się), `saveChangesError` parametr ma wartość true.

### <a name="the-delete-pages-onpostasync-method"></a>Metoda OnPostAsync stron Delete

Zastąp `OnPostAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Powyższy kod pobiera wybranej jednostki, następnie wywołuje [Usuń](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) metodę, aby ustawić stan jednostki `Deleted`. Gdy `SaveChanges` nosi SQL DELETE polecenia jest generowany. Jeśli `Remove` zakończy się niepowodzeniem:

* Zostaje przechwycony wyjątek bazy danych.
* Usuwać strony `OnGetAsync` metoda jest wywoływana z `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Strona Razor usuwania aktualizacji

Dodaj następujący wyróżniony komunikat o błędzie do strony Razor usunąć.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Przetestuj Delete.

## <a name="common-errors"></a>Typowe błędy

Nie działają dla uczniów/Index lub inne łącza:

Sprawdź stronę Razor zawiera poprawny `@page` dyrektywy. Na przykład strony Razor dla uczniów/indeksu powinna **nie** zawiera szablon trasy:

```cshtml
@page "{id:int}"
```

Musi zawierać każdej strony Razor `@page` dyrektywy.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/intro)
> [dalej](xref:data/ef-rp/sort-filter-page)
